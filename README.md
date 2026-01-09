<div align="center">
  <p align="center">
    <img alt="Tryhackme" src="https://repository-images.githubusercontent.com/518509014/f7450454-158c-45e0-8b38-0c0ae4d7394c" width="300px" />
    <h1> 🖥 Logging do Windows para SOC</h1>
    Este projeto detalha a metodologia utilizada para identificar um ataque de força bruta por protocolo RDP em ambiente Windows, utilizando logs de segurança.
  </p>
</div>

<br>

## 📑 Objetivos:
- Identificar tentativas de intrusão via Força Bruta;
- Analisar logs de acesso remoto (RDP) e movimentação lateral;
- Rastrear persistência (Backdoors).

<br>

## 🛠️ Ficha Técnica e Contexto Operacional:
- Plataforma: TryHackMe.
- Analista: [@Luanacyberdef](https://tryhackme.com/p/Luanacyberdef).
- Ambiente: Visualizador de Eventos (Event Viewer) e Logs do Sysmon.
- Metodologia: Detecção de intrusão e análise forense de artefatos de log.
- Sala no TryHackMe: [Logging do Windows para SOC](https://tryhackme.com/room/windowsloggingforsoc).

<br>

## 🔍 Investigação Prática no Event Viewer:
### 1. Detecção de Força Bruta RDP:
Com base na análise do log `Practice-Security.evtx`, segui a trilha forense para isolar a atividade maliciosa:

* **Identificação do IP de Origem:** Localizei o endereço IP externo responsável pelas tentativas massivas de conexão;

* **Usuário Alvo:** Identifiquei que o ataque focou na conta `A****`, confirmando a tentativa de quebra de privilégios elevados;

* **ID de Login RDP:** Extraí o identificador único de sessão `(0x*****)` no evento de sucesso, permitindo correlacionar as ações do invasor;

* **Análise de Hostname:** Verifiquei que o nome da estação de trabalho não seguia o padrão corporativo, um indicador clássico de dispositivo não autorizado na rede.

### 🏷 Recomendação:
- Habilitar NLA: Exigir Autenticação em Nível de Rede para forçar a validação antes da criação da sessão RDP;
- Bloqueio de Conta: Implementar políticas de bloqueio após sucessivas falhas de login (ex: 5 tentativas);
- Monitoramento de IPs: Bloquear automaticamente IPs de origem não esperados ou que apresentem comportamento anômalo de conexão.

<br>

### 2. Caça a Usuários Backdoor (Persistência):
Após confirmar o acesso malicioso via RDP, o foco mudou para identificar como o atacante garantiu permanência no sistema através da criação de contas não autorizadas.

* **Conta Criada:** Identifiquei que o atacante criou o usuário `sv******` imediatamente após o login RDP bem-sucedido;

* **Escalação de Privilégios:** O usuário backdoor foi adicionado a 2 grupos, garantindo acesso remoto persistente e capacidade de manipulação de dados;

* **Correlação de Sessão:** Confirmei que o campo ID de Logon da criação do usuário corresponde ao ID identificado na tarefa anterior `(0x*****)`, provando que a conta foi criada pelo invasor durante a mesma sessão maliciosa.

### 🏷 Recomendação:
- Revisão de Grupos Privilegiados: Auditoria periódica de membros nos grupos;
- Alertas de Criação de Contas: Implementar alertas imediatos para qualquer ocorrência de EID 4720, especialmente se originada de contas de serviço ou usuários não pertencentes ao RH/TI;
- Princípio do Menor Privilégio: Restringir a criação de usuários apenas a contas administrativas monitoradas.

<br>

### 3. Análise de Artefatos e Comunicação com Servidor de Comando e Controle:
Nesta etapa, utilizei os logs do Sysmon para rastrear a origem do malware e identificar a comunicação externa com a infraestrutura do atacante.

* **Vetor de Infecção:** Identifiquei que o usuário utilizou o navegador `GC` para acessar a internet;
* **Download Malicioso:** Rastreio do arquivo executável `c****.exe` baixado no diretório de Downloads do usuário;
* **Fonte da Ameaça:** O artefato foi recuperado da URL externa `http://**********3/c****.exe`;
* **Mecanismo de Persistência:** O malware criou um arquivo de atalho na pasta Startup do Windows para garantir a execução automática a cada reinicialização do sistema;
* **Comunicação Externa:** Detecção de conexão ativa com o servidor de Comando e Controle (C2) no endereço IP `193.**.***.*` na porta ****;
* **Domínio de Destino:** O IP malicioso foi correlacionado ao domínio `******.click`.

> **Nota:** O atacante usou uma porta não convencional para o C2.

### 🏷 Recomendação:
- Bloqueio de IoCs: Adicionar o IP `193.**.***.*` e o domínio `******.click` à lista de bloqueio do Firewall e Proxy corporativo;
- Varredura de Persistência: Implementar scripts de monitoramento para identificar novos arquivos em diretórios de Startup e chaves de registro Run/RunOnce;
- Filtragem de URL: Bloquear o acesso a domínios recém-registrados ou com nomes aleatórios, como o identificado na investigação.

---

## ⚠️ Nota de Ética e Integridade:
> [!IMPORTANT]
**Preservação da Experiência de Aprendizado:** Para garantir que outros profissionais e estudantes tenham uma experiência autêntica de investigação, as respostas diretas e artefatos específicos foram parcialmente ofuscados `(ex: http://**********3/c****.exe)`. O foco desta documentação é a metodologia analítica e o raciocínio técnico.

---

## 🏛️ Créditos e Direitos Autorais:
> [!IMPORTANT]
Este projeto foi desenvolvido como parte dos estudos práticos na plataforma [TryHackMe](https://tryhackme.com/). Todos os direitos sobre os laboratórios, marcas e infraestrutura de treinamento pertencem à respectiva plataforma. A documentação presente neste repositório reflete minha metodologia analítica e resultados obtidos durante a resolução do desafio.

---

<h2> 🔗 Compartilhe com a comunidade 🧡 </h2>

Por favor, se esse conteúdo te ajudou, não esqueça de compartilhar 😁

[![GitHub Repo stars](https://img.shields.io/badge/share%20on-twitter-03A9F4?logo=twitter)](https://twitter.com/share?url=https://github.com/Luanacyberdef/Analise-de-logs-com-EventViewer-e-Sysmon) [![GitHub Repo stars](https://img.shields.io/badge/share%20on-facebook-1976D2?logo=facebook)](https://www.facebook.com/sharer/sharer.php?u=https://github.com/Luanacyberdef/Analise-de-logs-com-EventViewer-e-Sysmon) [![GitHub Repo stars](https://img.shields.io/badge/share%20on-linkedin-3949AB?logo=linkedin)](https://www.linkedin.com/shareArticle?url=https://github.com/Luanacyberdef/Analise-de-logs-com-EventViewer-e-Sysmon)
