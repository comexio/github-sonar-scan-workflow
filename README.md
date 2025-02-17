# Documentação Técnica: Run Sonar Scan Workflow

Este workflow tem como objetivo executar uma análise de qualidade de código utilizando o SonarQube. O processo inclui:

•	Clonar o repositório para garantir acesso ao código-fonte.

•	Baixar relatórios do PHPUnit gerados previamente pelo workflow de testes unitários.

•	Corrigir caminhos no relatório do PHPUnit para garantir compatibilidade com o SonarQube.

•	Executar a análise com o SonarQube e enviar os dados para o servidor Sonar.

•	Aguardar a finalização do processamento no SonarQube.

•	Verificar o Quality Gate e determinar se a análise passou ou falhou.

•	Publicar um comentário no PR com o status do Quality Gate.

# Parâmetros de Entrada

**Autenticação**

•	CICD_GITHUB_TOKEN: Token de autenticação do GitHub, utilizado para interagir com o repositório.

•	SONAR_TOKEN: Token de autenticação para acesso ao SonarQube.

# Etapas do Workflow

1. Checkout do Código

Baixa o código do repositório para a máquina de execução.

2. Download dos Relatórios do PHPUnit

Baixa os relatórios de cobertura de testes unitários (coverage.xml e report.junit.xml), gerados pelo workflow de Run Unit Tests.

3. Extração dos Relatórios do PHPUnit

Descompacta o arquivo phpunit-reports.tar.gz, contendo os relatórios do PHPUnit.

4. Correção de Caminhos nos Relatórios

Corrige os caminhos dos arquivos nos relatórios para que sejam compatíveis com o SonarQube.

5. Envio dos Dados ao SonarQube

Executa a análise de código com o SonarQube, enviando:

•	Projeto baseado no nome do repositório (sonar.projectKey e sonar.projectName).

•	Relatórios de cobertura e qualidade de código.

6. Aguardo do Quality Gate

Pausa a execução por 15 segundos para garantir que o SonarQube processe os dados.

7. Verificação do Quality Gate

•	Consulta o resultado do Quality Gate no SonarQube.

•	Se houver falhas, o workflow será marcado como failed.

•	Caso esteja vinculado a um Pull Request, um comentário será adicionado no PR com os resultados.

# Considerações

•	Este workflow depende da execução prévia dos testes unitários para coletar relatórios de cobertura.

•	O Quality Gate do SonarQube pode ser configurado para definir limites de cobertura, duplicação de código e segurança.

•	Se o Quality Gate falhar, o pipeline será interrompido e marcado como erro.

•	O uso do SonarQube Hosted (SonarCloud) ou um servidor SonarQube on-premises pode ser configurado via SONAR_HOST_URL.