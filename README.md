# AVEVA-PI-System-Learning-Resource-Directory
Miniguia para aprendizagem sobre a interface de coleta OPC DA

**Contexto e Objetivo:**
**Aprender sober o sistema PI SYSTEM, mais voltado a configuração de coleta opc DA e melhores práticas.**

https://notebooklm.google.com/notebook/61b4a22f-9f1d-4a76-a525-a72ea61ef13f

Prompts:
você consegue gerar um passo a passo para a configuração de um opc da com a ordem de prioridade?
quais as melhores práticas para configurar uma interface opc da?
Qual a diferença entre polled tags e advise tags?
Gere um troubleshooting para possíveis erros na coleta de dados da interface de opc da

**1. Resumos Estruturados do Assunto**

**AVEVA PI Vision (Visualização de Dados)**
*   **O que é:** Uma ferramenta baseada em navegador web utilizada para construir displays e painéis (dashboards) que permitem visualizar, avaliar e monitorar dados operacionais do PI System em tempo real.
*   **Arquitetura e Requisitos:** A arquitetura envolve o Servidor de Aplicação (IIS), Banco de Dados SQL Server (para armazenar configurações e displays) e conexão com o PI Data Archive e PI AF Server. Os requisitos mínimos de hardware sugeridos (para 1 a 50 usuários) incluem 4 núcleos de CPU, 2 GHz de velocidade e 6 GB de RAM, além de instâncias do Windows Server (2016 a 2025) e SQL Server 2016 ou superior.
*   **Funcionalidades:** Permite a criação de coleções de símbolos, tabelas de séries temporais, gráficos e indicadores (gauges) com suporte a múltiplos estados (multi-states) baseados em valores limite para alertas visuais.

**PI Interface for OPC DA (Coleta de Dados)**
*   **O que é:** Aplicação cliente da AVEVA que se comunica entre um servidor OPC e o Data Archive para coletar dados em tempo real ou enviar dados de volta ao servidor OPC.
*   **Configuração Básica:** Realizada primariamente através do utilitário PI ICU (Interface Configuration Utility). O processo envolve criar a instância, configurar as permissões DCOM e os serviços Windows (regra de privilégio mínimo), estabelecer parâmetros de varredura (*scan classes*) e conectar ao servidor OPC de origem.
*   **Melhores Práticas:** Utilizar a versão *Read-Only* caso não haja necessidade de escrever de volta no sistema de controle; otimizar limites de grupos para no máximo 800 itens (sendo 200 muitas vezes mais eficiente na prática); e implementar *allowlists* para pontos de saída a fim de impedir edições acidentais no sistema de controle.

**Métodos de Coleta de Dados: Advise vs. Polled**
*   **Advise Points (Orientado a Evento):** O servidor OPC notifica a interface sempre que um novo valor está disponível em seu cache. São configurados na Classe de Varredura 1 (Scan Class 1) e definidos pelo atributo de Tag `Location3 = 1`.
*   **Polled Points (Orientado a Tempo):** A interface consulta o servidor OPC periodicamente baseada na taxa configurada em sua respectiva classe de varredura (Scan Class). Definidos pelo atributo `Location3 = 0`.
*   **Regra de Ouro:** Nunca misture *polled tags* e *advise tags* na mesma *scan class*.

**Alta Disponibilidade e Confiabilidade**
*   **Buffering (Armazenamento em Buffer):** Utiliza-se o PI Buffer Subsystem na máquina da interface para proteger e reter os dados em caso de falha de conexão da rede com o PI Data Archive, enviando-os posteriormente sem perdas.
*   **Failover (Redundância):** O sistema suporta failover no nível da interface (UniInt Failover, onde uma interface backup assume a coleta se a primária falhar) ou failover no nível do Servidor OPC (através do uso de "Watchdog tags" para avaliar a integridade do servidor de dados e alterar a conexão em caso de falha).

---

**2. Glossário de Conceitos Principais**

*   **PI Data Archive:** O núcleo do PI System. Servidor que armazena grandes volumes de dados de séries temporais gerados pelos sensores e dispositivos, disponibilizando-os para aplicativos cliente em alta velocidade.
*   **PI AF (Asset Framework):** Um repositório que estrutura os dados brutos de tempo real organizando-os em modelos lógicos e hierárquicos de ativos (ex: fábricas, unidades, equipamentos), provendo contexto para as medições.
*   **PI Point (Tag):** Um registro individual com marcação de tempo (time-stamp) armazenado no Data Archive. Cada tag rastreia uma medição contínua e única, como uma temperatura ou pressão de um instrumento.
*   **PI ICU (Interface Configuration Utility):** Interface gráfica e utilitário de sistema fundamental utilizado para configurar, gerenciar e dar suporte aos parâmetros de lote (.bat) e execução das interfaces de coleta.
*   **DCOM (Distributed Component Object Model):** Protocolo da Microsoft no qual os servidores e clientes OPC clássicos se baseiam para se comunicar através da rede. Requer atenção estrita a políticas de segurança e autenticação (ACLs) para evitar erros de bloqueio e falha de comunicação.
*   **ExcMax (Maximum exception reporting interval):** Parâmetro de configuração da tag que define o período máximo em segundos que o sistema pode aguardar antes de enviar novamente um valor ao Data Archive, mesmo que o valor do dado não tenha excedido a tolerância de mudança (deadband). Importante para atualizações gráficas forçadas no PI Vision.
*   **Watchdog Point:** Uma tag configurada para rastrear especificamente a disponibilidade ou integridade do servidor OPC de origem. Usado ativamente em arquiteturas de Failover para determinar se a conexão deve migrar para o servidor secundário.

---

**3. Conjunto de Prompts Reutilizáveis (Para Futuras Revisões)**

*Você pode copiar e colar estes prompts no futuro para focar ou expandir o seu aprendizado nas funcionalidades da suíte AVEVA / OSIsoft:*

1.  *"Detalhe o funcionamento e o processo de configuração do PI Buffer Subsystem em um ambiente que utiliza a Interface OPC DA para coleta de dados."*
2.  *"Quais são as diferenças técnicas, vantagens e desvantagens entre o Failover UniInt (nível de interface) e o Failover de nível de Servidor OPC?"*
3.  *"Estou enfrentando problemas de lentidão no servidor OPC resultando no erro 'The OPC server did not respond to the last refresh call'. Quais são as ações práticas sugeridas para otimizar o fluxo de tags e reduzir a sobrecarga?"*
4.  *"Descreva a estrutura de configuração DCOM necessária para uma comunicação segura e bem-sucedida entre o nó do cliente (Interface OPC DA) e o servidor OPC, incluindo os tipos de 'Impersonation Level' e 'Authentication Level'."*
5.  *"Liste o passo a passo para criar e editar um painel de monitoramento utilizando o AVEVA PI Vision, focando na aplicação do 'Asset Comparison Table' e em símbolos de multi-estado."*
6.  *"Explique a diferença entre os tratamentos de erro de rede e as configurações necessárias quando o PI OPC DA recebe dados classificados como de qualidade 'Questionable' ou 'Bad'."*
