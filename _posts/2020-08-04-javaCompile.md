---
layout: post
title: Java Compile
subtitle: Java Compile & JVM
---
# Java Build 과정
1. 생성한 Java File을 Build를 하게 된다.
    - Build 작업은 javac.exe의 명령어로 .class파일을 만드는 과정이다.
    - .class파일은 JVM에서 읽을 수 있는 java byteCode이다.
2. 이렇게 생성된 .class 파일은 `Class Loader`에 의해 JVM에 로드되고, 기계어로 해석되어 `Runtime Data Area`에 적재된다.
    - 이때 JVM은 byteCode를 해당 OS에 알맞게 기계어로 변환합니다.
    - byteCode를 실행하는 엔진은 `Interpreter`, `JIT Compiler`가 존재합니다. 이 둘은 추후에 정리
3. 이후 각 class, method, 변수들은 각각 알맞는 Runtime Data Area에 할당되게 됩니다.
4. 이후 static 영역에 위치한 main method를 실행하게됩니다.

# JVM의 구조
JVM의 5가지 영역으로 구분됩니다.
1. Stack Area
2. Static Area
3. Heap Area
4. PC Register Area
5. Native Method Stack Area

### Stack Area
method를 실행했을 때 사용되는 `parameter`, `variable`, `returnValue`등이 저장되며 `Last-in-First-out` 의 방식으로 method 실행 시 마다 데이터가 적재됩니다.

method의 실행이 끝나면 저장공간은 삭제됩니다.

### Static Area
Class의 정보, static method, static variable 등의 정보가 저장됩니다.

JVM에 byteCode가 적재되면서 데이터가 저장되기 때문에 객체의 생성없이도 static method, static variable를 사용가능합니다.

### Heap Area
객체가 생성되면 객체의 주소값이 저장되는 공간입니다.

GC의 대상이 되는 영역입니다.

모든 객체는 Heap Area에 생성이 되고, stack Area에서는 heap 영역의 주소값이 가지고 참조하게 됩니다.

### PC Register Area
Thread마다 하나씩 생성되는 공간입니다.

### Native Method Stack Area
Java이외의 다른 언어의 method를 호출을 위해 할당되는 영역입니다.

## GC(Garbage Collector)
Heap 영역에 생성된 객체의 정보를 자동으로 정리해주는 역할을 합니다.

GC가 없다면 C언어처럼 메모리 할당과 해제를 개발자가 손수 관리해야합니다.

GC가 할당된 영역을 해제하는 방법은 `reachability`라는 개념을 사용합니다.

### Heap영역의 세부 내용
Heap 영역의 세부적으로 Eden, Survivor1, Survivor2, Old, Permanent 영역으로 구분됩니다.

#### Eden
객체가 처음 생성되면 위치하는 곳이 Eden영역입니다.

### Survivor1, Survivor2
객체들이 생성되다 보면 참조 정도에 따라 Eden 영역, 그리고 서로의 Survivor 영역에서 이동됩니다.

### Old 
Survivor 영역에서 일정 횟수동안 이동하게 되면, Old영역으로 이동하게 됩니다.

## Minor GC
Eden영역, Survivor 영역에서 일어나는 GC입니다.

Eden 영역에 일정 수치의 객체가 쌓이게 되면 Survivor 영역으로 이동하게 되는데 이떄 참조가 없는 객체를 삭제하게 됩니다.

이 객체가 삭제되는 과정을 Minor GC라고 합니다.

## Major GC
Old영역에서 이루어지는 GC로써, Old영역이 꽉차게 되면 발생하게 됩니다.

이떄 Old영역에 남아 있는 모든 객체들을 전부 검사하여 삭제하게 되는데, 이 과정에서 Application이 잠시 정지되게 됩니다.

# main method가 static인 이유 
Java에서 method를 실행하려면 객체가 heap 영역에 생성이 되어야하고, stack 영역에 실행한 method가 위치해야 합니다.

하지만 main class는 아직 heap 영역에 할당 되지 않았습니다.

그렇기 때문에 main method는 static method여야 합니다.

static method는 static 영역에 미리 위치하게 됩니다.

static 영역에 위치했다는것은 heap영역에 객체를 생성하지 않고 사용이 가능합니다.

가장 먼저 실행이 되어야하는 main method는 static 영역에 위치하여 객체의 생성없이 바로 사용이 가능합니다. 

