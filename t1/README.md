# Entendendo o Princípio da Responsabilidade Única
Juliana Karoline de Sousa - RA 594997

## O que é SOLID?
SOLID é um conjunto de princípios utilizado no paradigma de orientação a objetos que ajuda na escrita de códigos flexíveis e de fácil manutenção.

Esses princípios foram definidos por Robtert C. Martin, também conhecido como Uncle Bob. Atualmente SOLID pode ser considerado como parte da metodologia ágil de desenvolvimento.

## Por que usar SOLID?
Desenvolver software sem planejamento ou sem padrões de design pode resultar num código passível de bugs e altamente acoplado. 

Dar manutenção nesse tipo de sistema se torna uma tarefa complexa, dado que qualquer modificação pode acarretar em problemas em outras partes do projeto.

Seguir os princípios SOLID é uma boa prática de desenvolvimento para evitar esses problemas, além de facilitar a refatoração de código e permitir criar códigos estensíveis.

## S - Princípio da Responsabilidade Única

O primeiro princípio SOLID, representado pela letra S, é o Princípio da Responsabilidade Única (SRP):

> Uma classe deve ter um, e somente um, motivo para mudar.

O SRP implica que cada classe deve possuir apenas uma responsabilidade. Desse modo, a classe só deverá ser modificada caso a responsabilidade dela seja mudada.

Separando responsabilidades diferentes em classes diferentes diminui a chance de mudanças interferirem em outras partes do código, além de diminuir o acoplamento das classes.

## Exemplo de aplicação do SRP
Para demonstrar a aplicação do SRP, considere a classe Robot. O robô possui um motor, que pode ser um servo motor ou um motor DC. Além disso, o robô pode andar para frente ou parar de se movimentar.

```python
class Robot:
    def __init__(self, motor_type, pin):
        if motor_type == 'servo':
            self.motor = PWM(Pin(pin))
        elif motor_type == 'dc:
            self.motor = GPIO(Pin(pin))
        self.motor.type = motor_type

    def go_forward():
        if self.motor.type == 'servo':
            self.motor.set_angle(90)
        elif self.motor.type == 'dc:
            self.motor.output = 'HIGH'

    def stop():
        if self.motor.type == 'servo':
            self.motor.set_angle(0)
        elif self.motor.type == 'dc':
            self.motor.output = 'LOW'
```
Nessa implementação, a classe Robot não segue o SRP, pois além da movimentação ela também está responsável por lidar com a lógica dos motores.

Desse modo, caso o robô precise utilizar um motor de passo, por exemplo, toda a lógica de movimentação do robô precisaria ser reescrita.

Para separar as responsabilidades, podemos implementar duas classes para encapsular a manipulação dos motores. 

Considere as implementações das classes MotorServo e MotorDC.

```python
class MotorServo:
    def __init__(self, pin):
        self.motor = PWM(Pin(pin))

    def forward():
        self.motor.set_angle(90)
    
    def stop():
        self.motor.set_angle(0)

class MotorDC:
    def __init__(self, pin):
        self.motor = GPIO(Pin(pin))

    def forward():
        self.motor.output = 'HIGH'
    
    def stop():
        self.motor.output = 'LOW'
```
As duas classes lidam diretamente com os motores, expondo os métodos de movimentação para serem utilizados pela classe Robot.

Podemos reescrever a classe Robot utilizando as classes MotorServo e MotorDC. Assim, para movimentar o robô basta chamar os métodos de movimentação dos motores.

```python
class Robot:
    def __init__(self, motor_type, pin):
        if motor_type == 'servo':
            self.motor = MotorServo(pin)
        elif motor_type == 'dc':
            self.motor = MotorDC(pin)

    def go_forward():
        self.motor.forward()

    def stop():
        self.motor.stop()
```

No construtor da classe Robot é instanciado o tipo de motor utilizado no robô. A partir da instanciação, as funções de movimento do robô não precisam saber o tipo de motor utilizado, dado que os motores provém a mesma interface.

Portanto, a classe Robot passa a ter apenas uma responsabilidade: movimentar o robô. Do mesmo modo, as classes MotorServo e MotorDC são responsáveis apenas pela comunicação com os motores.