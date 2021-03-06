# Central Processing Unit

- 컴퓨터 시스템에서 가장 중요한 일을 하는 부분으로서 프로그램을 수행하는 장치로 명령어를 수행하며, 그 명령어의 수행 순서를 제어하는 기능을 가지고 있는 장치다.
- CPU의 구분
    - 스택 구조 CPU : 연산 대상체나 결과를 스택에 넣어두고 운용하는 CPU로 수식을 Postfix 표기법으로 변환 후, 0주소 명령어 형식을 사용하여 연산하게 된다.
    - 단일 누산기 구조 CPU : 연산 대상체나 결과를 누산기(Accumulator)에 넣고 운용하는 CPU로, 1주소 명령어 형식을 사용하게 된다.
    - 범용 레지스터 구조 CPU : 연산 대상체나 결과를 레지스터나 메모리에 넣어두고 운용하는 CPU로, 2주소 명령어 형식을 사용하게 된다.
- 구성요소
    - 산술/논리연산장치(ALU: Arithmetic and Login Unit)
    - 레지스터 세트
    - 제어장치(Control Unit)
    - 내부버스

<br>

## 처리장치(Processing Unit)

- 데이터를 처리하는 연산을 실행.
- 연산장치(산술/논리연산장치)와 레지스터, 내부버스들로 구성.
    - 산술/논리연산장치(ALU: Arithmetic and Login Unit)는 산술연산, 논리연산, 비트연산 등의 연산을 수행한다. 가산기, 누산기(Accumulator), 자리 올림 플립플롭, 오버플로 체크 플립플롭, 보수기 등으로 구성되어 있다.
    - 레지스터는 연산에 사용되는 데이터를 저장하거나 연산의 결과를 저장.
    - 연산장치는 독립적으로 데이터 처리를 수행하지 못하며 반드시 레지스터들과 조합하여 데이터를 처리한다.

### 마이크로 연산(micro-operation)

- **CPU에서 발생시키는 하나의 클록 펄스(Clock Pulse)동안 실행되는 기본 동작(연산)**을 의미한다.
- CPU에 있는 레지스터와 플래그의 상태 변환을 일으키게 하는 동작을 의미한다.
- CPU에서 발생시키는 제어 신호에 따라 마이크로 오퍼레이션이 순서적으로 일어난다.
- 명령어 수행은 마이크로 오퍼레이션의 수행으로 이루어진다.
- 마이크로연산의 일반적인 형태는 다음과 같다.
    1. 레지스터 전송 마이크로연산 : 한 레지스터에서 다른 레지스터로 2진 데이터를 전송하는 연산
    2. 산술(arithmetic) 마이크로연산 : 레지스터 내의 데이터에 대해서 실행되는 산술연산
        - 덧셈, 뺄셈, 1 증가, 1 감소, 보수연산
    3. 논리(logic) 마이크로연산 : 레지스터 내의 데이터에 대한 비트를 조작하는 연산
        - AND, OR, XOR, NOT 연산
        - 레지스터에 저장되어 있는 비트의 데이터를 조가하는 데 유용
    4. 시프트(shift) 마이크로연산 : 레지스터 내의 데이터를 시프트시키는 연산

<br>

## 제어장치(Control Unit)

- **특정한 데이터 연산을 실행할 수 있도록 처리장치(Processing Unit)에 마이크로연산을 구동시키는 여러 가지 신호를 제공하는 장치**이다.
- 제어장치는 **외부 입력신호인 컴퓨터 명령어를 받아들여 이 신호에 해당하는 마이크로연산을 처리장치로 내보낸다.**
- 처리장치에서는 이 마이크로연산에 의해 입력 데이터를 받아들여 데이터를 처리한 후, 그 결과를 출력한다. 이때 보내지는 상태신호는 처리장치에서의 연산결과로 나타난 상태들이며, 분기나 조건판단 등을 위한 판단기준을 제공해 주는 신호이다.
- 구성요소
    - 명령어 레지스터
    - 명령어 해독기
    - 주소처리기
    - 순서제어기

### 명령어 수행과정

1. PC(Program Counter)에 저장된 주소를 메모리 주소 레지스터(MAR)로 보낸다.
2. MAR에 저장된 주소에 있는 기억장치의 명령어를 메모리 버퍼 레지스터(MBR)로 읽어 오고, 프로그램 카운터를 1 증가시킨다. 또한 명령어 수행을 위해 읽어 온 명령어를 명령어 레지스터(IR)에 저장한다.
3. 명령어 연산코드와 오퍼랜드로 구성되어 있으며, 연산코드(OP code)는 명령어 해독기로 보내고 오퍼런드(Operand)는 주소처리기로 보낸다.
4. 주소처리기는 메모리 주소 레지스터(MAR)를 통해 명령어 수행에 필요한 오퍼런드의 주소 또는 다음 명령어의 주소를 계산하여, 이를 위해 기억장치에 접근한다.
5. 제어신호 발생기에서는 위의 1~4까지의 과정에서 필요한 제어신호와 연산 코드를 해독하여 명령어 수행을 위한 제어신호를 발생시킨다. 이때 만약 명령어가 점프 명령어와 같이 기억장치의 주소를 참조하는 명령어라면 제어신호 발생기는 주소처리기를 동작시켜 다음에 수행될 명령어의 주소를 계산한다.
6. 현재 명령어 레지스터에 있는 명령어의 수행이 끝나면 증가된 PC의 내용은 다음에 수행할 명령어의 주소를 지정하거나, 점프 명령어와 같이 분기가 필요한 경우에는 해당 명령어의 주소를 지정하는데 이것은 다시 1부터의 과정을 반복하며 실행된다.

<br>

![https://user-images.githubusercontent.com/88426509/146635044-18e77e4d-25f6-431d-94f2-8adbb1528aec.png](https://user-images.githubusercontent.com/88426509/146635044-18e77e4d-25f6-431d-94f2-8adbb1528aec.png)

[https://www.youtube.com/watch?v=wJ8KMceX8ic&list=PLimVTOIIZt2YUccM4iDH54nSepCVJLDQj&index=22](https://www.youtube.com/watch?v=wJ8KMceX8ic&list=PLimVTOIIZt2YUccM4iDH54nSepCVJLDQj&index=22)


## 레지스터

범용 레지스터와 특수 레지스터가 있다.

### 범용 레지스터

- 여러 가지 연산을 위해 데이터 저장, 주소 저장과 같은 일반적인 목적을 위해 사용되는 레지스터이다.
- CPU 내부에 있는 소규모의 일시적인 기억장치로 프로그램의 진행 도중 가까운 시간 내에 사용할 데이터나 연산결과를 일시적으로 기억시키는 데 사용한다.
- 데이터를 연산할 때 메모리로부터 데이터를 인출할 경우 호출시간이 많이 걸리기 때문에 CPU 내부의 레지스터에 데이터를 기억시켜 두고 연산한다.

### 프로그램 카운터(PC: Program Counter)

- 프로그램 카운터는 데이터가 저장되어 있는 기억장치의 주소를 지정해 주는 레지스터이다.
- 현재 처리하려고 하는 데이터를 인출한 후에는 자동적으로 1 증가하여 다음에 수행해야 할 명령어가 저장된 기억장치주소를 지정해 준다.
- 프로그램 카운터의 비트수는 CPU가 지정할 수 있는 기억장치의 전체 영역을 지정할 수 있어야 되기 때문에 기억장치의 용량에 따라 비트수가 결정된다. 예를 들어 기억장치 전체 영역이 256MByte(=<!-- $2^{28}$ --> <img style="transform: translateY(0.1em); background: white;" src="https://render.githubusercontent.com/render/math?math=2%5E%7B28%7D">)라면 프로그램 카운터는 28비트가 된다.

### 누산기(AC: Accumulator)

- 데이터를 일시 저장하는 레지스터이다.
- 입력장치로부터 데이터를 받아들이거나 출력장치로 데이터를 전송하는 데 사용한다.
- 산술연산 및 논리연산이 이루어질 경우에는 오퍼랜드나 연산결과를 일시적으로 기억하는 레지스터이다.
- CPU가 연산을 수행한 후 그 결과는 반드시 누산기에 저장된다.

### 기억장치주소 레지스터(MAR: Memory Address Register)

- 기억장치주소를 일시 저장하는 레지스터

### 스택 포인터(SP: Stack Pointer)

- 스택 주소지정방식에서 사용한다.
- 스택 영역의 번지를 지정해 주는 포인터이다.
- 스택 영역은 실제로 데이터가 피신되는 기억장소로서 기억장치에 위치한다.
- 프로그램 카운터와 같은 크기의 비트수를 가진다.

### 명령어 레지스터(IR: Instruction Register)

- 프로그램 수행 중에 기억장치로부터 가장 최근에 인출된 명령어를 갖고있다.
- 명령어 레지스터가 가지는 비트수는 명령어 연산코드의 비트수와 같다.

### 기억장치 버퍼 레지스터(MBR: Memory Buffer Register)

- 기억장치로 쓰일 데이터나 기억장치로부터 읽힐 데이터를 임시로 저장하는 레지스터

<br>

## 명령어 사이클(instruction cycle)

1개의 명령어를 CPU에서 수행하는 데 필요한 전체 수행 과정

### 명령어 사이클의 종류

1. 명령어 인출 사이클(fetch cycle)
    - 명령어의 인출 사이클은 기억장치에 기억되어 있는 명령어를 인출하는 과정이다.
    - 명령어 인출 사이클을 마이크로연산 표현을 이용하여 살펴보면 다음과 같은 세 단계로 이루어진다.
        - T(0) : MAR ← PC
        - T(1) : MBR ← M[MAR], PC ← (PC) + 1
        - T(2) : IR ← MBR
    - 따라서 명령어 인출 사이클은 3개의 CPU 클록 시간이 필요하다.
2. 명령어 실행 사이클(execute cycle)
    - 명령어 실행 사이클은 명령어를 실행하는 단계이다.
    - 이 과정에서는 명령어 인출과정을 통하여 IR 레지스터에 실린 명령어를 해독하고, 해독한 명령어에 따라 필요한 연산이 수행된다.
    - 아래는 LOAD 명령어의 실행 사이클 동안의 마이크로연산 표현이다.
        - T(0) : MAR ← IR(adrs)
        - T(1) : MBR ← M[MAR]
        - T(2) : AC ← MBR
3. 간접 사이클(indirect cycle)
    - 간접 사이클은 간접주소지정방식을 사용하는 명령어에서 오퍼랜드 부분의 유효주소를 결정하는 데 사용한다.
    - 간접 사이클의 마이크로연산은 다음과 같으며, 3개의 클록으로 완성된다.
        - T(0) : MAR ← IR(adrs)
        - T(1) : MBR ← M[MAR]
        - T(2) : IR ← MBR
4. 인터럽트 사이클(interrupt cycle)
    - 인터럽트 사이클은 CPU의 정상적인 동작 중에 입출력장치로부터의 인터럽트 처리 요청이 발생하였을 때 실행
    - 인터럽트란 CPU가 현재 처리 중인 프로그램 루틴을 중단시키고 다른 동작을 수행하도록 하는 것을 말함.
    - 인터럽트 사이클의 마이크로 연산
        - T(0) : MBR ← PC
        - T(1) : MAR ← SP, PC ← ISR(adrs)
        - T(2) : M[MAR] ← MBR

<br>

## 명령어 파이프라이닝(instruction pipelining)

병렬처리(parallel processing)는 컴퓨터 시스템의 계산속도 향상을 목적으로 동시에 데이터 처리기능을 제공하는 광범위한 개념의 기술을 의미한다. 병렬처리의 방법에는 여러 가지가 있는데 그중 파이프라인(pipeline) 처리는 하나의 프로세스를 서로 다른 기능을 가진 여러 개의 서브프로세스로 나누어 각 서브프로세스가 동시에 서로 다른 데이터를 취급하도록 하는 기법이다. 이러한 원리는 명령어 수행 단계에서도 적용될 수 있는데 이것을 명령어 파이프라이닝(pipelining)이라고 한다.

컴퓨터에서 파이프라인 구조는 CPU의 처리속도를 향상시키기 위한 방법 중의 하나로서, **CPU의 내부 하드웨어를 여러 단계로 나누어 처리**하는 기술을 명령어 파이프라이닝(instruction pipelining)이라고 한다. **즉, 하나의 처리기(processor)를 여러 개의 부 처리기(sub-processor)로 나누어 각각의 처리기가 동시에 서로 다른 데이터를 처리하는 구조**로서, **1개의 명령어를 수행하는 데 각 단계별로 병렬처리**가 가능하여 CPU의 속도를 빠르게 할 수 있다. 이러한 명령어 파이프라이닝을 명령어 수행 사이클에 적용하면 여러 개의 명령어가 중첩되어 실행되도록 구현할 수 있으며, 수행 단계별로 2단계, 4단계, 6단계 명령어 파이프라인이 있다. 이론적으로는 파이프라인에서 명령어 사이클을 여러 단계로 나눌수록 동시에 많은 명령어가 처리될 수 있다.

### 2단계 명령어 파이프라인

- 명령어 수행 사이클은 명령어 인출 단계와 실행 단계라는 2개의 독립적인 파이프라인 모듈로 분리하여 수행하는 방법이다.

### 4단계 명령어 파이프라인

![https://upload.wikimedia.org/wikipedia/commons/thumb/c/cb/Pipeline%2C_4_stage.svg/375px-Pipeline%2C_4_stage.svg.png](https://upload.wikimedia.org/wikipedia/commons/thumb/c/cb/Pipeline%2C_4_stage.svg/375px-Pipeline%2C_4_stage.svg.png)

[https://upload.wikimedia.org/wikipedia/commons/thumb/c/cb/Pipeline%2C_4_stage.svg/375px-Pipeline%2C_4_stage.svg.png](https://upload.wikimedia.org/wikipedia/commons/thumb/c/cb/Pipeline%2C_4_stage.svg/375px-Pipeline%2C_4_stage.svg.png)

- 명령어 수행 단계를 4단계, 즉 명령어 인출(IF), 명령어 해독(ID), 오퍼런드 인출(OF), 실행 단계(EX)로 나누어 수행하는 방법이다.
- 실행 과정을 보면 두 번째 클록 주기에서부터 명령어 단계가 중첩되어 병렬로 처리, 최대 4개의 단계가 동시에 처리되고 있다.

### 6단계 명령어 파이프라인

- 명령어 수행을 여섯 단계로 나누어 수행하는 방법이다.
    - 명령어 인출(Fetch Instruction)
    - 명령어 해독(Decode Instruction)
    - 오퍼랜드를 계산(Calculate Operand)
    - 오퍼랜드를 인출(Fetch Operand)
    - 명령어를 실행(Execute Instruction)
    - 연산된 결과 오퍼랜드를 저장(Write Operand)

<br>

## 프로세서의 종류

### CISC: Complex Instruction Set Computer(복합 명령어 집합 컴퓨터)

복합명령어를 포함하여 명령어와 주소지정방식의 수를 많이 사용하는 구조의 컴퓨터

- 명령어 집합
    - 명령어에 따라 다양한 길이를 갖는 가변길이 명령어 형식
    - 명령어 수행시간이 1클록에서 많게는 수백 클록까지 소요
    - 약 200여 개 이상의 명령어
- 제어장치의 구성
    - 마이크로프로그램에 의한 제어방식으로 제어장치를 구성
- 레지스터 구조
    - CPU 내부에 범용 레지스터의 수가 적다
    - 기억장치에 있는 데이터를 엑세스하기 위해서는 기억장치 접근이 자주 발생하며, 이는 컴퓨터 성능저하의 요인이 된다.
- 파이프라인의 적용 효율
    - 명령어의 길이가 가변적이기 때문에 각 단계별 처리시간을 동일하게 해 주기가 어려우므로 파이프라인 구조에서는 비효율적이다.

### RISC: Reduced Instruction Set Computer(단축 명령어 집합 컴퓨터)

간단한 명령어와 최소한의 주소지정방식을 사용하는 구조의 컴퓨터

- 명령어 집합
    - 고정길이의 명령어 형식을 사용하기 때문에 명령어 집합이 단순
    - 모든 명령어가 1워드 단위로 고정되어 있기 때문에 1개 명령어를 수행하는 데 한개 클록만 소요됨.
    - 약 30여 개의 명령어만 가지고 있다.
- 제어장치의 구성
    - 하드웨어에 의한 제어방식으로 제어장치를 구성한다.
- 레지스터 구조
    - CPU 내부에 많은 수(약 32개에서 200여 개)의 범용 레지스터가 있다.
    - 처리하고자 하는 데이터를 미리 CPU의 내부 레지스터로 가져와서 실행할 수 있으므로 처리속도의 향상을 도모할 수 있다.
- 파이프라인의 적용 효율
    - 명령어가 고정길이 명령어이기 때문에 각 단곔별 처리시간을 동일하게 해주기가 용이하므로 파이프라인 구조에 효율적이다.

### 수퍼 스칼라 RISC

- 파이프라인 병렬 처리가 가능하게 한 구조이다.
- RISC 구조의 확장형으로 명령어의 효율적인 처리를 위해 고안된 구조이다.

### VLIW(Very Long Instruction Word)

- 수평 마이크로 명령 형식을 사용하므로 명령어의 해독이 필요 없으며, 비트 하나가 하나의 명령을 의미한다.
- 하나의 명령어의 길이가 수백 비트(128~512Bit)로 크다.
