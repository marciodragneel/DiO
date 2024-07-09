import aspectlib

# Função transversal (aspecto)
@aspectlib.Aspect
def log_function_call(pointcut):
    with aspectlib.weave(pointcut) as ctx:
        print(f"Calling {ctx._args[0].__name__} with args {ctx._args[1:]}")
        try:
            yield
            print(f"{ctx._args[0].__name__} executed successfully")
        except Exception as e:
            print(f"Exception {e} occurred in {ctx._args[0].__name__}")
            raise e

# Funções alvo onde o aspecto será aplicado
def funcao_alvo1(x, y):
    return x + y

def funcao_alvo2(a, b, c):
    return a * b / c

# Aplicando o aspecto às funções alvo
with log_function_call.before(pointcut=f"call({funcao_alvo1})"):
    resultado1 = funcao_alvo1(3, 4)
    print("Resultado da função 1:", resultado1)

with log_function_call.before(pointcut=f"call({funcao_alvo2})"):
    resultado2 = funcao_alvo2(5, 6, 2)
    print("Resultado da função 2:", resultado2)
