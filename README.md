# UiPath - Processamento e Validação de Faturas PDF

Solução de **Robotic Process Automation (RPA)** desenvolvida no **UiPath Studio** para automação end-to-end do processamento, leitura via Regex, validação de regras fiscais de negócio e consolidação de faturas em PDF.

O projeto foi construído com arquitetura modular e boas práticas de desenvolvimento RPA, servindo tanto para aplicação no ambiente corporativo quanto para portfólio prático de preparação para a **Certificação UiPath**.

---

## Objetivos do Projeto

- **Integração Cloud (Google Drive)**: Download automático do pacote de faturas PDF da nuvem.
- **Extração Inteligente de Dados**: Leitura de PDFs e extração precisa de campos via Expressões Regulares (Regex).
- **Motor de Validação Fiscal**: Checagem rigorosa de regras de negócio para identificação de inconsistências.
- **Consolidação e Auditoria**: Exportação dos resultados em planilha Excel com separação em abas de faturas válidas e inválidas.
- **Rastreabilidade**: Geração de logs detalhados em cada etapa da execução.

---

## Arquitetura do Projeto

O projeto adota o padrão de desenvolvimento modular no UiPath (Invoke Workflow File):

- **Main.xaml**: Orquestrador principal da automação
- **Leitura_PDFs.xaml**: Módulo de leitura do PDF e extração via Regex
- **Validacao_Faturas.xaml**: Módulo de aplicação das regras de negócio
- **Consolidacao_Excel.xaml**: Módulo de geração e formatação do relatório Excel
- **Relatorio_Faturas_Consolidado.xlsx**: Relatório final gerado

---

## Fluxo de Execução e Módulos

### 1. Main.xaml (Orquestrador)
- Gerencia o ciclo de vida do processo e tratamento de exceções.
- Prepara diretórios de trabalho e baixa o pacote de faturas PDF da nuvem.
- Invoca sequencialmente os submódulos de leitura, validação e consolidação.

### 2. Leitura_PDFs.xaml (Extração de Dados)
- Executa a atividade Read PDF Text para cada fatura.
- Utiliza Regex ancorado para extração precisa:
  - Numero_Fatura
  - Data_Emissao
  - CNPJ do Emissor
  - Razão Social
  - Valor Total
- Popula a DataTable bruta (out_dtFaturasBrutas).

### 3. Validacao_Faturas.xaml (Motor de Regras)
Aplica validações sequenciais em cada linha:
- CNPJ: Checa se está em branco ou fora do padrão numérico de CNPJ.
- Número da Fatura: Identifica campos ausentes, nulos ou S/N.
- Valor Financeiro: Verifica se o valor é menor ou igual a zero.
- Decisão Única: Se boolEValida e verdadeiro, encaminha para dtFaturasValidas; caso contrário, direciona para dtFaturasInvalidas com o motivo do erro.

### 4. Consolidacao_Excel.xaml (Relatório)
- Gera o arquivo Excel com duas abas:
  - Faturas_Validas: Lista limpa de faturas aprovadas.
  - Faturas_Invalidas: Lista de exceções com a descrição da inconsistência.
- Faz o upload do relatório final para o Google Drive.

---

## Regras de Negócio e Validações

- **CNPJ Ausente / Inválido**: Texto em branco, ISENTO ou fora do padrão de CNPJ -> Marca como inválida com motivo CNPJ inválido ou ausente.
- **Sem Número de Fatura**: Campo em branco ou preenchido como S/N -> Marca como inválida com motivo Número da fatura ausente.
- **Valor Inválido / Zerado**: Valor menor ou igual a R$ 0,00 -> Marca como inválida com motivo Valor zerado ou inválido.

---

## Impacto e Ganho de Negócio

- Redução do Tempo: Dezenas de faturas lidas, validadas e consolidadas em segundos.
- Zero Erro Humano: Precisão na validação fiscal e financeira.
- Auditoria Simplificada: Gestão por exceção com justificativas claras.
- Escalabilidade: Capacidade de absorver picos de demanda no fechamento mensal.

---

## Tecnologias Utilizadas

- UiPath Studio
- UiPath.PDF.Activities
- UiPath.Excel.Activities
- UiPath.GSuite.Activities
- VB.NET e Regular Expressions (Regex)

---

## Autor

Desenvolvido como projeto prático de automação RPA e consolidação de conhecimento para a Certificação UiPath.
