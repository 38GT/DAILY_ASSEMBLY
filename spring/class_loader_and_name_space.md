<h1 style="font-size: 48px;">A class loader has its own namespace</h1>

# JAVA CODES
```java
package basic.of.spring;

public class ClassLoaderTest {
    public static void main(String[] args) {
        String classPath = "build/classes/java/main/";

        // 두 개의 CustomClassLoader를 사용하여 Singleton 클래스를 로드
        CustomClassLoader customClassLoader1 = new CustomClassLoader(classPath);
        CustomClassLoader customClassLoader2 = new CustomClassLoader(classPath);

        try {
            // 첫 번째 클래스 로더로 Singleton 클래스 로드
            Class<?> singletonClass1 = customClassLoader1.findClass("basic.of.spring.Singleton");
            Object singletonInstance1 = singletonClass1.getMethod("getInstance").invoke(null);

            // 두 번째 클래스 로더로 Singleton 클래스 로드
            Class<?> singletonClass2 = customClassLoader2.findClass("basic.of.spring.Singleton");
            Object singletonInstance2 = singletonClass2.getMethod("getInstance").invoke(null);

            // 두 개의 인스턴스 비교
            System.out.println("Class loaded by customClassLoader1: " + singletonClass1.getName());
            System.out.println("Class loaded by customClassLoader2: " + singletonClass2.getName());

            System.out.println("Instance 1 hashCode: " + System.identityHashCode(singletonInstance1));
            System.out.println("Instance 2 hashCode: " + System.identityHashCode(singletonInstance2));

            // 싱글톤 인스턴스가 동일한지 여부 확인
            if (singletonInstance1 == singletonInstance2) {
                System.out.println("Singleton instance is the same.");
            } else {
                System.out.println("Singleton instances are different.");
            }

            Singleton singleton1 = Singleton.getInstance();
            Singleton singleton2 = Singleton.getInstance();

            System.out.println(System.identityHashCode(singleton1));
            System.out.println(System.identityHashCode(singleton2));

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}


```

# RESULTS
```bash
Class loaded by customClassLoader1: basic.of.spring.Singleton
Class loaded by customClassLoader2: basic.of.spring.Singleton
Instance 1 hashCode: 1199823423
Instance 2 hashCode: 2128227771
Singleton instances are different.
1996181658
1996181658
```