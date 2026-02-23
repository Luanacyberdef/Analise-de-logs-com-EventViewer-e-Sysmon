<div align="center">
    <img alt="Tryhackme" src="https://repository-images.githubusercontent.com/518509014/f7450454-158c-45e0-8b38-0c0ae4d7394c" width="300px" />
    <h1> 🖥 Logging do Windows para SOC</h1>
    Este projeto detalha a metodologia utilizada para identificar um ataque de força bruta por protocolo RDP em ambiente Windows, utilizando logs de segurança.
</div>

<br>

## 📑 Objetivos:
- Identificar tentativas de intrusão via Força Bruta;
- Analisar logs de acesso remoto (RDP) e movimentação lateral;
- Rastrear persistência (Backdoors) e comunicação C2.

<br>

## 🛠️ Ficha Técnica e Contexto Operacional:
- Plataforma: TryHackMe.
- Analista: [@Luanacyberdef](https://tryhackme.com/p/Luanacyberdef).
- Ambiente: Visualizador de Eventos (Event Viewer) e Logs do Sysmon.
- Metodologia: Detecção de intrusão e análise de artefatos de log.
- Sala no TryHackMe: [Logging do Windows para SOC](https://tryhackme.com/room/windowsloggingforsoc).

<br>

## 🖥️ Visão Geral do Ambiente:
<img width="678" height="606" alt="image" src="https://github.com/user-attachments/assets/510e8972-a399-4154-b3ad-1f259fa2ab51" />

> **Nota:** Visão simulada do ambiente Windows analisado

<br>

## ⚠️ Nota de Ética e Integridade:
> [!WARNING]
> **Preservação da Experiência de Aprendizado:** Alguns artefatos, identificadores e URLs foram parcialmente ofuscados com o objetivo de preservar a experiência de aprendizado de outros estudantes e profissionais, mantendo a integridade do material educacional.

<br>

## 🔍 Investigação Prática no Event Viewer:
### 🚨 Detecção de Força Bruta RDP
Com base no log `Practice-Security.evtx`, foram identificadas tentativas massivas de login:

- **IP de Origem:** Endereço externo responsável pelas tentativas;
- **Usuário Alvo:** `A****` foco do ataque;
- **ID de Login RDP:** `(0x*****)` correlação de sessão;
- **Hostname:** Dispositivo não autorizado (fora do padrão corporativo).

### Recomendações:
- Habilitar NLA (Network Level Authentication) para RDP;
- Bloqueio de conta após 5 tentativas falhas;
- Monitoramento de IPs suspeitos ou comportamento anômalo.

<br>

### 🚨 Caça a Usuários Backdoor (Persistência)
Após acesso via RDP, verificou-se criação de contas não autorizadas:

- **Conta Criada:** `sv******`;
- **Escalação de Privilégios:** Adicionada a 2 grupos administrativos;
- **Correlação de Sessão:** ID de logon corresponde à sessão RDP maliciosa `(0x*****)`.
 
### Recomendações: 
- Auditoria periódica de membros de grupos privilegiados;  
- Alertas para qualquer criação de usuário (EID 4720);  
- Aplicar princípio do menor privilégio: criação restrita a contas monitoradas.

<br>

### 🚨 Análise de Artefatos e Comunicação C2
Investigação via Sysmon para rastrear malware e comunicação externa:

- **Vetor de Infecção:** `Download via Navegador`; 
- **Arquivo Malicioso:** `c****.exe`;
- **URL de Origem:** `http://**********3/c****.exe`;
- **Persistência:** Atalho na pasta Startup;
- **Conexão C2:** IP `193.**.***.*` e domínio `******.click`.

> **Nota:** O atacante usou uma porta não convencional para o C2.

### Recomendações:
- Bloquear IP e domínio no firewall;
- Monitorar diretórios de Startup e chaves de registro; 
- Filtrar URLs suspeitas ou recém-registradas.

---

## 🏛️ Créditos e Direitos Autorais:
> [!WARNING]
> Este projeto faz parte de estudos práticos na plataforma [TryHackMe](https://tryhackme.com/). <br>
> Todos os direitos sobre laboratórios, marcas e infraestrutura pertencem à respectiva plataforma. <br>
> A documentação reflete a metodologia analítica e os resultados obtidos durante a resolução do desafio.

## 🤖 Uso de IA:
> [!NOTE]
> Parte deste conteúdo foi elaborada com apoio de ferramenta de IA, utilizada como auxílio na organização do texto, com revisão e validação integral pelo autor.

---

## 🔗 Compartilhe com a comunidade 🧡

Se esse conteúdo te ajudou, não esqueça de compartilhar 😁

[![GitHub Repo stars](https://img.shields.io/badge/Compartilhe%20no-Linkedin-blue?logo=Linkedin)](https://www.linkedin.com/shareArticle?url=https://github.com/Luanacyberdef/Analise-de-logs-com-EventViewer-e-Sysmon) [![GitHub Repo stars](https://img.shields.io/badge/Compartilhe%20no-Reddit-%23ff5700?logo=Reddit&logoColor=white)](https://www.reddit.com/submit?url=https://github.com/Luanacyberdef/Analise-de-logs-com-EventViewer-e-Sysmon) [![GitHub Repo stars](https://img.shields.io/badge/Compartilhe%20no-Twitter-black?logo=x&logoColor=white)](https://twitter.com/share?url=https://github.com/Luanacyberdef/Analise-de-logs-com-EventViewer-e-Sysmon) [![GitHub Repo stars](https://img.shields.io/badge/Compartilhe%20no-Facebook-%231976D2?logo=Facebook)](https://www.facebook.com/sharer/sharer.php?u=https://github.com/Luanacyberdef/Analise-de-logs-com-EventViewer-e-Sysmon)