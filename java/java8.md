## mixin with default method
```java
public interface Cat {
    String getCatKind();
    String getFurDescription();
    
    default void growl() {
        System.out.println("Grrrrowl!!");
    }
    default void walk() {
        System.out.println(getCatKind() + " is walking.");
    }
    default void eat() {
        System.out.println(getCatKind() + " is eating.");
    }
    default void sleep() {
        System.out.println(getCatKind() + " is sleeping.");
    }
}

public interface Meowler {
  default void meow() {
    System.out.println("MeeeeeOww!");
  }
}

public interface Purrable {
    default void purr() {
        System.out.println("Purrrrrrr...");
    }
}

public interface Roarable {
    default void roar() {
        System.out.println("Roar!!");
    } 
}

public class HouseCat implements Cat, Purrable, Meowler {
    @Override
    public String getCatKind() {
        return "Domestic Cat";
    }

    @Override
    public String getFurDescription() {
        return "mixed brown and white";
    }
}

public class Tiger implements Cat, Roarable {

    @Override
    public String getCatKind() {
        return getClass().getSimpleName();
    }

    @Override
    public String getFurDescription() {
        return "striped";
    }
}

public class Cheetah  implements Cat, Purrable {

    @Override
    public String getCatKind() {
        return getClass().getSimpleName();
    }

    @Override
    public String getFurDescription() {
        return "spotted";
    }
    
}

public class Mixins {
    public static void main(String[] args) {
        Tiger bigCat = new Tiger();
        Cheetah mediumCat = new Cheetah();
        HouseCat smallCat = new HouseCat();
        
        System.out.printf("%s with %s fur.\n", bigCat.getCatKind(), bigCat.getFurDescription());
        bigCat.eat();
        bigCat.sleep();
        bigCat.walk();
        bigCat.roar();
        bigCat.growl();
        System.out.println("------------------");
        
        System.out.printf("%s with %s fur.\n", mediumCat.getCatKind(), mediumCat.getFurDescription());
        mediumCat.eat();
        mediumCat.sleep();
        mediumCat.walk();
        mediumCat.growl();
        mediumCat.purr();
        System.out.println("------------------");
        
        System.out.printf("%s with %s fur.\n", smallCat.getCatKind(), smallCat.getFurDescription());
        smallCat.eat();
        smallCat.sleep();
        smallCat.walk();
        smallCat.growl();
        smallCat.purr();
        smallCat.meow();
        System.out.println("------------------");
        
    }
}
```
