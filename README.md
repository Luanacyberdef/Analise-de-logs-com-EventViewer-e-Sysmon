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

## 🖥️ Visão Geral do Dashboard:
<img width="678" height="606" alt="image" src="https://github.com/user-attachments/assets/510e8972-a399-4154-b3ad-1f259fa2ab51" />

> **Nota:** Visão simulada do ambiente Windows analisado

<br>

## 🔍 Investigação Prática no Event Viewer:
### 1. Detecção de Força Bruta RDP
Com base no log `Practice-Security.evtx`, foram identificadas tentativas massivas de login:

- **IP de Origem:** Endereço externo responsável pelas tentativas;
- **Usuário Alvo:** `A****` foco do ataque;
- **ID de Login RDP:** `(0x*****)` correlação de sessão;
- **Hostname:** Dispositivo não autorizado (fora do padrão corporativo).

### 🏷 Recomendações:
- Habilitar NLA (Network Level Authentication) para RDP;
- Bloqueio de conta após 5 tentativas falhas;
- Monitoramento de IPs suspeitos ou comportamento anômalo.

<br>

### 2. Caça a Usuários Backdoor (Persistência)
Após acesso via RDP, verificou-se criação de contas não autorizadas:

- **Conta Criada:** `sv******`;
- **Escalação de Privilégios:** Adicionada a 2 grupos administrativos;
- **Correlação de Sessão:** ID de logon corresponde à sessão RDP maliciosa `(0x*****)`.
 
### 🏷 Recomendações: 
- Auditoria periódica de membros de grupos privilegiados;  
- Alertas para qualquer criação de usuário (EID 4720);  
- Aplicar princípio do menor privilégio: criação restrita a contas monitoradas.

<br>

### 3. Análise de Artefatos e Comunicação C2
Investigação via Sysmon para rastrear malware e comunicação externa:

- **Vetor de Infecção:** `Pelo Navegador`; 
- **Arquivo Malicioso:** `c****.exe`;
- **URL de Origem:** `http://**********3/c****.exe`;
- **Persistência:** Atalho na pasta Startup;
- **Conexão C2:** IP `193.**.***.*` e domínio `******.click`.

> **Nota:** O atacante usou uma porta não convencional para o C2.

### 🏷 Recomendações:
- Bloquear IP e domínio em firewall/proxy;
- Monitorar diretórios de Startup e chaves de registro Run/RunOnce; 
- Filtrar URLs suspeitas ou recém-registradas.

---

## ⚠️ Nota de Ética e Integridade:
> [!WARNING]
> **Preservação da Experiência de Aprendizado:** Artefatos e URLs foram parcialmente ofuscados para manter a integridade do aprendizado sem comprometer a segurança.

---

## 🏛️ Créditos e Direitos Autorais:
> [!IMPORTANT]
> **Nota:** Este projeto faz parte de estudos práticos na plataforma [TryHackMe](https://tryhackme.com/).
> Todos os direitos sobre laboratórios, marcas e infraestrutura pertencem à respectiva plataforma.
> A documentação reflete a metodologia analítica e os resultados obtidos durante a resolução do desafio.

---

<h2> 🔗 Compartilhe com a comunidade 🧡 </h2>

Por favor, se esse conteúdo te ajudou, não esqueça de compartilhar 😁

[![GitHub Repo stars](https://img.shields.io/badge/share%20on-twitter-03A9F4?logo=twitter)](https://twitter.com/share?url=https://github.com/Luanacyberdef/Analise-de-logs-com-EventViewer-e-Sysmon) [![GitHub Repo stars](https://img.shields.io/badge/share%20on-facebook-1976D2?logo=facebook)](https://www.facebook.com/sharer/sharer.php?u=https://github.com/Luanacyberdef/Analise-de-logs-com-EventViewer-e-Sysmon) [![GitHub Repo stars](https://img.shields.io/badge/share%20on-linkedin-3949AB?logo=linkedin)](https://www.linkedin.com/shareArticle?url=https://github.com/Luanacyberdef/Analise-de-logs-com-EventViewer-e-Sysmon)
