import logging
import abc

# Настройка логирования
logging.basicConfig(level=logging.INFO)

class ComplexNumber:
    def __init__(self, real=0, imag=0):
        self.real = real
        self.imag = imag

    def __str__(self):
        return f"{self.real} + {self.imag}i"

class OperationStrategy(metaclass=abc.ABCMeta):
    @abc.abstractmethod
    def execute(self, num1, num2):
        pass

class AdditionStrategy(OperationStrategy):
    def execute(self, num1, num2):
        result = ComplexNumber(num1.real + num2.real, num1.imag + num2.imag)
        logging.info(f"Выполнено сложение: {num1} + {num2} = {result}")
        return result

class MultiplicationStrategy(OperationStrategy):
    def execute(self, num1, num2):
        real = num1.real * num2.real - num1.imag * num2.imag
        imag = num1.imag * num2.real + num1.real * num2.imag
        result = ComplexNumber(real, imag)
        logging.info(f"Выполнено умножение: {num1} * {num2} = {result}")
        return result

class DivisionStrategy(OperationStrategy):
    def execute(self, num1, num2):
        denom = num2.real ** 2 + num2.imag ** 2
        real = (num1.real * num2.real + num1.imag * num2.imag) / denom
        imag = (num1.imag * num2.real - num1.real * num2.imag) / denom
        result = ComplexNumber(real, imag)
        logging.info(f"Выполнено деление: {num1} / {num2} = {result}")
        return result

class ComplexCalculator:
    def __init__(self, strategy):
        self.strategy = strategy

    def set_strategy(self, strategy):
        self.strategy = strategy

    def execute_operation(self, num1, num2):
        return self.strategy.execute(num1, num2)

# Пример использования
calc = ComplexCalculator(AdditionStrategy())
num1 = ComplexNumber(1, 1)
num2 = ComplexNumber(2, 2)
result = calc.execute_operation(num1, num2)
print(result)
