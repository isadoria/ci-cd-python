# Pipeline CI/CD com Python e GitHub Actions

Projeto prático da disciplina de Garantia de Software, com um pipeline de
Continuous Integration e Continuous Delivery configurado via GitHub Actions.

## Estrutura do projeto

- `calculadora.py` — funções de soma, subtração, multiplicação e divisão
- `test_calculadora.py` — testes automatizados com pytest
- `requirements.txt` — dependências do projeto
- `.github/workflows/pipeline.yml` — definição do pipeline CI/CD

## Perguntas

### 1. O que representa a etapa de CI neste projeto?

A etapa de Continuous Integration representa a garantia automática de que
toda alteração enviada ao repositório passa pelos testes antes de qualquer
possibilidade de entrega. Ela funciona como um *quality gate*: a cada push
ou pull request na branch `main`, o job `ci` baixa o código, configura o
Python, instala as dependências e executa os testes com `pytest`. Se algum
teste falhar, o código não é considerado apto a avançar no pipeline.

### 2. O que impede a execução do Continuous Delivery quando existe um defeito?

A linha `needs: ci` no job `delivery` estabelece uma dependência direta em
relação ao job `ci`. O GitHub Actions só executa o job `delivery` se o job
`ci` terminar com sucesso. Assim, se qualquer teste falhar, o job `ci`
recebe status de falha e o `delivery` é automaticamente bloqueado, impedindo
que um código com defeito seja empacotado ou disponibilizado como artefato.

### 3. Qual seria a próxima etapa necessária para transformar este pipeline em Continuous Deployment?

Atualmente o pipeline realiza apenas o Continuous Delivery: ele gera e
publica um artefato (`calculadora-python`) pronto para ser entregue, mas a
implantação em si ainda depende de uma ação manual. Para evoluir para
Continuous Deployment, seria necessário adicionar um novo job (ou passos
adicionais no job `delivery`) que realizasse o deploy automático desse
artefato em um ambiente real — por exemplo, publicando em um servidor, em
um serviço de nuvem (AWS, Azure, Heroku etc.) ou em um container — sem
qualquer intervenção humana no processo, completando o ciclo de entrega
totalmente automatizado.

## Evidências

- Execução bem-sucedida do pipeline: jobs `Continuous Integration` e
  `Continuous Delivery` com status ✅.
- Demonstração do bloqueio de delivery: commit introduzindo uma regressão
  proposital em `somar()`, causando falha em `test_somar` e impedindo a
  execução do job `delivery` (`needs: ci`).
- Artefato gerado: `calculadora-python`, contendo `calculadora.py` e
  `build-info.txt` com o hash do commit.
