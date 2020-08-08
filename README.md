# Pytest pt_br

> Baseado no documento oficial https://buildmedia.readthedocs.org/media/pdf/pytest/latest/pytest.pdf

> A intenção dessa "tradução" é, além de estudar, prover material para quem queira buscar o pytest em português do Brasil.

<!-- toc -->

- [Instalação e primeiros passos](#instalação-e-primeiros-passos)
  * [Instalar pytest](#instalar-pytest)
  * [Criando o primeiro teste](#criando-o-primeiro-teste)
  * [Executando vários testes](#executando-vários-testes)
  * [Verificando que uma determinada exceção é levantada](#verificando-que-uma-determinada-exceção-é-levantada)
  * [Agrupando vários testes em uma classe](#agrupando-vários-testes-em-uma-classe)
  * [Solicitando um diretório temporário exclusivo para testes funcionais](#solicitando-um-diretório-temporário-exclusivo-para-testes-funcionais)
  * [Continue lendo](#continue-lendo)
  
- [Usage and Invocations](#heading-1)
  * [Invocando pytest através de python -m pytest](#sub-heading-1)
  * [Códigos de saída possíveis](#sub-heading-1)
  * [Obtendo ajuda em versão, nomes de opções, variáveis de ambiente](#sub-heading-1)
  * [Parando após a primeira (ou N) falhas](#sub-heading-1)
  * [Especificando/selecionando testes](#sub-heading-1)
  * [Modificando o Python traceback printing](#sub-heading-1)
  * [Summary report detalhado](#sub-heading-1)
  * [Executando no PDB (Python Debugger) em falhas](#sub-heading-1)
  * [Executando no PDB (Python Debugger) no início de um teste](#sub-heading-1)
  * [Colocando Breakpoints](#sub-heading-1)
  * [Usando a função de breakpoint interna](#sub-heading-1)
  * [Capturando a duração de testes](#sub-heading-1)
  * [Manipulador de falhas](#sub-heading-1)
  * [Criando arquivos nos formatos JUnitXML](#sub-heading-1)
  * [Criando arquivos nos formatos resultlog](#sub-heading-1)
  * [Enviando relatórios de testes para serviços pastebin online](#sub-heading-1)
  * [Plugins de carregamento antecipado](#sub-heading-1)
  * [Desabilitando plugins](#sub-heading-1)
  * [Executando pytest atráves de código Python](#sub-heading-1)




## Instalação e primeiros passos

Versões: Python 3.5, 3.6, 3.7, 3.8, 3.9, PyPy3
Plataformas: Linux and Windows
Nome do pacote PyPI: pytest
Documentatação como PDF: download latest
pytest é um framework que torna a criação de testes simples e escaláveis fácil. Testes são expressivos e legíveis - não é necessário uso de código boilerplate. Dê início em minutos com um pequeno teste unitário ou testes funcionais complexos para sua aplicação.


### Instalar pytest

Execute o comando abaixo no seu terminal:
> pip install -U pytest

Verifique se a versão foi instalada corretamente:
> pytest --version

> pytest 6.0.1


### Criando o primeiro teste

Crie uma função de teste simples com apenas 4 linhas de código:

```python
# content of test_sample.py
def func(x):
  return x + 1
  
def test_answer():
  assert func(3) == 5
```

É isso, agora você pode executar esse teste:
```
pytest
=========================== test session starts ============================
platform linux -- Python 3.x.y, pytest-6.x.y, py-1.x.y, pluggy-0.x.y
cachedir: $PYTHON_PREFIX/.pytest_cache
rootdir: $REGENDOC_TMPDIR
collected 1 item
test_sample.py F [100%]
================================= FAILURES =================================
_______________________________ test_answer ________________________________
def test_answer():
   assert func(3) == 5
 E assert 4 == 5
>E + where 4 = func(3)
test_sample.py:6: AssertionError
========================= short test summary info ==========================
FAILED test_sample.py::test_answer - assert 4 == 5
============================ 1 failed in 0.12s =============================
```
O 100% refere-se ao progresso total dos testes executados. Quando termina, o pytest, então, exibe um relatório de erro devido à func(3) não retornar 5.

**Nota:** Você pode usar a declaração de assert para verificar expectativas no teste. A asserção avançada do pytest vai reportar valores intermediários para
que você possa evitar os muitos nomes dos métodos do JUnit legado.


### Executando vários testes
```
pytest vai executar todos os arquivos da forma test_*.py ou *_test.py no diretório atual em seus subdiretórios. Mas
geralmente, segue as regras de descoberta de teste padrão.
```

### Verificando que uma determinada exceção é levantada

Utilize o _raise_ helper para verificar que determinado código levanta uma exceção:

```python
# content of test_sysexit.py
import pytest

def f():
  raise SystemExit(1)

def test_mytest():
  with pytest.raises(SystemExit):
    f()
```

Execute o teste com o modo de relatório _quiet_:
```
$ pytest -q test_sysexit.py
. [100%]
1 passed in 0.12s
```

**Nota:** O parâmetro -q/--quiet mantém a saída sucinta neste e nos próximos testes.


### Agrupando vários testes em uma classe

Quando você desenvolve vários testes, existe a possibilidade de agrupá-los em uma classe. O pytest torna a atividade de criar uma classe contendo mais de um teste bem fácil.
```python
# content of test_class.py
class TestClass:
  def test_one(self):
    x = "this"
    assert "h" in x
    
  def test_two(self):
    x = "hello"
    assert hasattr(x, "check")
```
O pytest descobre os testes seguindo a Convenção do Python para descobrimento de testes, sendo assim, executa ambas as funções que começam com test_.
Não é necessário uso de subclasse ou algo do gênero, mas, tenha certeza que o nome de sua classe deve começar com o nome Test, senão seus testes serão
pulados. Podemos executar esse módulo apenas passando o nome do arquivo como parâmetro:

```
# content of test_class.py
$ pytest -q test_class.py
.F [100%]
================================= FAILURES =================================
____________________________ TestClass.test_two ____________________________
self = <test_class.TestClass object at 0xdeadbeef>
  def test_two(self):
    x = "hello"
>     assert hasattr(x, "check")
E     AssertionError: assert False
E     + where False = hasattr('hello', 'check')
test_class.py:8: AssertionError
========================= short test summary info ==========================
FAILED test_class.py::TestClass::test_two - AssertionError: assert False
1 failed, 1 passed in 0.12s
```

O primeiro teste passou e o segundo falhou. Você pode observar facilmente os valores intermediários na asserção para ajudá-lo a entender a razão da falha.
Agrupar testes em classes pode ser benéfico pelas seguintes razões:
• Organização do teste
• Compartilhar fixtures específicas para testes naquela classe particular
• Aplicando comportamentos à nível de classe e tendo-os aplicados implicitamente nos testes

Algo a ficar atento é quando agrupamos testes em uma classe éque cada teste é uma instância única daquela classe. Com cada teste
dividindo a mesma instância de classe seria prejudicial ao isolamento do teste e iria promover práticas ruins de testes. Isso é 
exibido abaixo:
```python
# content of test_class_demo.py
class TestClassDemoInstance:
  def test_one(self):
    assert 0
  def test_two(self):
    assert 0
```

```
$ pytest -k TestClassDemoInstance -q
FF [100%]
================================= FAILURES =================================
______________________ TestClassDemoInstance.test_one ______________________
self = <test_class_demo.TestClassDemoInstance object at 0xdeadbeef>
  def test_one(self):
> assert 0
E assert 0
test_class_demo.py:3: AssertionError
______________________ TestClassDemoInstance.test_two ______________________
self = <test_class_demo.TestClassDemoInstance object at 0xdeadbeef>
  def test_two(self):
> assert 0
E assert 0
test_class_demo.py:6: AssertionError
========================= short test summary info ==========================
FAILED test_class_demo.py::TestClassDemoInstance::test_one - assert 0
FAILED test_class_demo.py::TestClassDemoInstance::test_two - assert 0
2 failed in 0.12s

```


### Solicitando um diretório temporário exclusivo para testes funcionais

O pytest nos dá acesso à builtin fixtures/functions para solicitar recursos diversos, como, por exemplo, diretório temporário:
```python
# content of test_tmpdir.py
def test_needsfiles(tmpdir):
  print(tmpdir)
  assert 0
```

Adicione o nome tmpdir na assinatura da função de teste e o pytest irá procurá-lo e chamar uma fixture factory para criar o recurso antes da chamada da função de teste.
Antes dessa execução, o pytest cria um diretório temporário único-por-teste:

```
$ pytest -q test_tmpdir.py
F [100%]
================================= FAILURES =================================
_____________________________ test_needsfiles ______________________________
tmpdir = local('PYTEST_TMPDIR/test_needsfiles0')
  def test_needsfiles(tmpdir):
    print(tmpdir)
>   assert 0
E   assert 0
test_tmpdir.py:3: AssertionError
--------------------------- Captured stdout call ---------------------------
PYTEST_TMPDIR/test_needsfiles0
========================= short test summary info ==========================
FAILED test_tmpdir.py::test_needsfiles - assert 0
1 failed in 0.12s
```
Mais informações sobre como usar tmpdir estão disponíveis na sessão de **diretórios temporários e arquivos**.
Veja que tipo de fixtures internas o pytest disponibiliza com o comando:

> pytest --fixtures # shows builtin and custom fixtures

**Nota:** O comando omite fixtures que começam com ```__``` a menos que a opção **-v** seja utilizada.
### Continue lendo
Veja recursos adicionais do pytest que podem ajudálo a customizar seus testes:

• “Invocando pytest através do comando python -m pytest” 

• “Usando pytest em uma suíte de teste existente”

• “Marcando funções de testes com atributos” para uso do pytest.mark

• “pytest fixtures: explícito, modular, escalável”

• “Escrevendo plugins”

• “Boas práticas de integração” para virtualenv e test layouts

