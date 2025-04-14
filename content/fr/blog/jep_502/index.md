---
title: 'JDK25 : Initialisez des objets immuables avec les StableValue'
date: 2025-04-11T18:20:06+02:00
draft: false
---

# Introduction

Les `StableValue` sont une nouvelle fonctionnalité introduite dans la version _preview_ de la JDK 25, visant à améliorer la gestion des objets immuables en Java. Cette innovation permet de traiter certaines valeurs dynamiques comme des constantes, offrant ainsi des optimisations de performance similaires à celles des champs `final`, tout en offrant une plus grande flexibilité quant au moment de leur initialisation.

La JEP liée à cette évolution est la [JEP-502](https://openjdk.org/jeps/502), dont les objectifs sont définis ainsi :

- Améliorer le démarrage des applications Java en **fragmentant l'initialisation** monolithique de l'état de l'application.
- Découpler la **création** des valeurs stables de leur **initialisation**, sans pénalités de performance significatives.
- Garantir que les valeurs stables sont initialisées au plus **une fois**, même dans les programmes **multi-threadés**.
- Permettre au code utilisateur de bénéficier en toute sécurité des optimisations de **constant folding**[^1], au même titre que les variables déclarées `final`

## Qu'est-ce qu'une Stable Value ?

```java
public sealed interface StableValue<T>
```

Une `StableValue` est un objet encapsulant une donnée immuable. Contrairement aux champs `final`, qui doivent être initialisés dès la création de l'objet ou de la classe, les Stable Value peuvent être initialisés à la demande, après leur déclaration. Cela signifie qu'ils ne sont définis qu'au moment où leur valeur est nécessaire, ce qui peut améliorer les performances de démarrage des applications Java -en évitant de charger l'état complet de l'application à son démarrage-.

## Exemples d'utilisation

> Tous les exemples suivants sont tirés de la JavaDoc officielle[^2].


Voici un exemple simple d'utilisation des `StableValue` dans une classe `OrderController`.

### Version classique :

```java
class OrderController {

    private final Logger logger = Logger.create(OrderController.class);

    void submitOrder(User user, List<Product> products) {
        logger.info("order started");
        //...
        logger.info("order submitted");
    }

}
```
Dans cette version, le logger est instancié automatiquement chaque fois qu'une instance de la classe `OrderController` est créée. Cela pourrait se traduire par un ralentissement de la création d'une instance de cette classe, puisque l'on peut imaginer que la création de ce logger soit relativement lente (si elle nécessite de lire un fichier de config par exemple, ou de préparer un fichier de sortie pour les logs...)

### Version StableValue

```java
class OrderController {

    private final StableValue<Logger> logger = StableValue.of();

    Logger getLogger() {
        return logger.orElseSet(() -> Logger.create(OrderController.class));
    }

    void submitOrder(User user, List<Product> products) {
        getLogger().info("order started");
        // ...
        getLogger().info("order submitted");
    }
}
```

Dans cet exemple, le champ `logger` est une `StableValue`. Il est initialisé uniquement lorsque la méthode `getLogger()` est appelée pour la première fois. Cela permet de retarder l'initialisation coûteuse du logger jusqu'à ce qu'il soit réellement nécessaire.

## Fonction stable
Les `StableValue` posent les fondations d'abstractions de plus haut niveau. Ainsi, un `supplier` permet par exemple de produire une valeur et de mettre en cache son résultat : 

```java
 public class Component {

     private final Supplier<Logger> logger =
             StableValue.supplier( () -> Logger.getLogger(Component.class) );

     public void process() {
        logger.get().info("Process started");
        // ...
     }
 }
```
On notera, dans ce cas de figure, l'absence d'accesseur au logger par rapport à l'exemple précédent, ce qui rend son utilisation plus élégante. 

## Collection stable
De la même façon, il est possible de créer des `Collection` stables, dont les valeurs sont calculées lors du premier accès : 

```java
final class PowerOf2Util {

    private PowerOf2Util() {}

    private static final int SIZE = 6;
    private static final IntFunction<Integer> ORIGINAL_POWER_OF_TWO =
            v -> 1 << v;

    private static final List<Integer> POWER_OF_TWO =
            StableValue.list(SIZE, ORIGINAL_POWER_OF_TWO);

    public static int powerOfTwo(int a) {
        return POWER_OF_TWO.get(a);
    }
}

int pwr4 = PowerOf2Util.powerOfTwo(4);
```
> N.B. la JVM transformera directement cette valeur en 16 au bout de quelques cycles d'exécution, grâce au constant folding

## Composition
Enfin, il est également possible de faire de la composition de Stable Value, ce qui permet d'évaluer paresseusement des objets : 

```java
 public final class DependencyUtil {

     private DependencyUtil() {}

     public static class Foo {
          // ...
      }

     public static class Bar {
         public Bar(Foo foo) {
              // ...
         }
     }

     private static final Supplier<Foo> FOO = StableValue.supplier(Foo::new);
     private static final Supplier<Bar> BAR = StableValue.supplier(() -> new Bar(FOO.get()));

     public static Foo foo() {
         return FOO.get();
     }

     public static Bar bar() {
         return BAR.get();
     }

 }
```

L'appel à `bar()` créera le singleton `Bar` s'il n'est pas déjà créé. Lors de cette création, le dépendant `Foo` sera d'abord créé si `Foo` n'existe pas déjà.

## Thread Safety

Le contenu d'une valeur stable est garanti d'être défini au plus une fois. Si plusieurs threads sont en compétition pour définir une valeur stable, une seule mise à jour réussit (le premier thread qui arrive à initialiser la valeur), tandis que les autres mises à jour sont bloquées jusqu'à ce que la valeur stable soit définie.

## Essayer par soi-même
Pour essayer cette fonctionnalité, téléchargez la [jdk25](https://jdk.java.net/25/) puis compilez et lancer votre programme en activant les preview :

```bash
# Compilation
javac --release 25 --enable-preview Main.java
# Exécution
java --enable-preview Main
```

## Conclusion

Les Stable Value représentent un mécanisme particulièrement intéressant dans la gestion des objets immuables en Java. En offrant une initialisation différée et des optimisations de performance, ils permettent aux développeurs de créer des applications plus efficaces et réactives. 


[^1]: Le constant folding [avec javac](https://fekir.info/post/compile-time-optimizations-in-java/#_optimization-done-by-javac) et avec [le JIT](https://jojozhuang.github.io/programming/java-advanced-jit-compiler/)
[^2]: https://cr.openjdk.org/~pminborg/stable-values2/api/java.base/java/lang/StableValue.html