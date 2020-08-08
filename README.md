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
> pytest

> =========================== test session starts ============================

> platform linux -- Python 3.x.y, pytest-6.x.y, py-1.x.y, pluggy-0.x.y

> cachedir: $PYTHON_PREFIX/.pytest_cache

> rootdir: $REGENDOC_TMPDIR

> collected 1 item

> test_sample.py F [100%]

> ================================= FAILURES =================================

> _______________________________ test_answer ________________________________

> def test_answer():

> assert func(3) == 5

> E assert 4 == 5

> >E + where 4 = func(3)

> test_sample.py:6: AssertionError

> ========================= short test summary info ==========================

> FAILED test_sample.py::test_answer - assert 4 == 5

> ============================ 1 failed in 0.12s =============================

### Executando vários testes

This is an h2 heading

### Verificando que uma determinada exceção é levantada

This is an h2 heading

### Agrupando vários testes em uma classe

This is an h2 heading

### Solicitando um diretório temporário exclusivo para testes funcionais

This is an h2 heading

### Continue lendo

This is an h2 heading

