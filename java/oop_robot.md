<h1 style="font-size: 48px;">OOP Robot</h1>


1. interface는 코드들의 느슨한 결합을 만들어내고 싶을 때에 사용한다.
2. abstract class를 정의함으로 공통된 구체적인 메서드를 만들어낼 수 있다.

```java

package org.example;

import com.sun.tools.jconsole.JConsoleContext;

public class Main {
    public static void main(String[] args) {

        fusion_device fusion_device = new fusion_device();
        LeftRobot leftRobot = new LeftRobot();
        RightRobot rightRobot = new RightRobot();

        fusion_device.enroll(leftRobot, rightRobot);
        fusion_device.assemble();
    }
}

abstract class Robot {
    public abstract void assemble();
}

class LeftRobot extends Robot {
    String mode;
    public void assemble(){
        this.mode = "leftMode";
        System.out.println("Left Robot Assemble! " + this.mode);
    }
}

class RightRobot extends Robot {
    String mode;
    public void assemble(){
        this.mode = "RightMode";
        System.out.println("Right Robot Assemble! " + this.mode);
    }
}

class fusion_device {
    private RightRobot rightRobot;
    private LeftRobot leftRobot;

    public void enroll(LeftRobot leftRobot, RightRobot rightRobot) {
        this.leftRobot = leftRobot;
        this.rightRobot = rightRobot;
    }
    public void assemble(){
        this.rightRobot.assemble();
        this.leftRobot.assemble();
    }
}


```