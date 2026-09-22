# Política de Segurança — MC Network Analyzer

> **Última atualização:** 2026-09-22 · Aplica-se a todas as releases publicadas em `mirocontrol/MCNA-Dist`.

---

## 1. Relatando uma Vulnerabilidade de Segurança

🔴 **NÃO ABRA ISSUES PÚBLICAS** para vulnerabilidades de segurança. Filtro humano, sem rastreamento público, sem PoC.

### Canal Cifrado Oficial:

**E-mail:** `security@mirocontrol.com.br`  
**PGP Key (opcional, se você tiver chave):** Solicite a chave pública PGP por e-mail.

O e-mail **deve conter**:
1. Descrição clara da vulnerabilidade (Impacto / Severidade)
2. Passos reproduzíveis (PoC mínimo)
3. Versão MCNA afetada (`mc-analyzer --version` ou tela de Sobre)
4. Ambiente: SO, Npcap versão, tipo de interface Ethernet
5. Exfiltração de dados: confirme se **nenhum dado** de cliente/piloto foi usado

---

## 2. Acordo de Divulgação Coordenada (Responsável)

Ao reportar, você concorda com:

| Compromisso Miro Control | Compromisso Pesquisador |
|--------------------------|-------------------------|
| Acusar recebimento em até **48h úteis** | Não publicar PoC em redes sociais / blogs / CVE pública antes da data acordada |
| Atribuir crédito (nome/empresa) em release notes (se quiser) | Não explorar a vulnerabilidade em redes OT reais |
| Patch de segurança em até **60 dias úteis** (Crítica: 30 dias) | Compartilhar detalhes apenas com o time de segurança |
| **Recompensa ($) para Critical/High**: Bounty B2B (negociado por e-mail, dependendo de impacto) | Não exfiltra dados de planta / cliente / engenheiro |

---

## 3. Escopo da Política (o que se qualifica como "vulnerabilidade MCNA")

### ✅ Qualificam:

| Categoria | Exemplo |
|-----------|---------|
| RCE Execução de código remoto | Uvicorn/FastAPI deserialização que leva a execução no endpoint `/api/*` |
| Escalação privilégio local | Falha no launcher CMD `.cmd` que eleva UAC sem consentimento |
| Colisão / DDoS em planta | Pacote malformado Modbus RTU que crasha app e trava porta COM do PC |
| Leak de segredos | Segredo de licensing HWID codado hardcoded em `.exe` |
| Bypass Read-Only | Operação de escrita (Modbus FC06 / DCP Set / CIP Static IP) executada sem código 6 dígitos + dupla confirmação |
| Chain of Trust quebrada | Hash de release no `hashes.txt` diverge do ZIP (corrupção intencional) |

### ❌ Não qualificam (encaminhe para suporte normal):

| Categoria | Canal correto |
|-----------|---------------|
| Bug de UI que não tem impacto de segurança | Issues públicos do MCNA-Dist |
| Falha de rede por driver de placa (não MCNA) | Suporte técnico WhatsApp/email |
| Pergunta sobre LGPD / política de privacidade | E-mail corporativo `mc-analyzer@mirocontrol.com.br` |
| Problemas de compilação de código | **Repo privado `MC-NetworkAnalyzer` (devs autorizados)** |

---

## 4. Versões Suportadas com Patches de Segurança

| Versão | Status | Suporte Segurança até |
|--------|--------|-----------------------|
| `0.17.x-beta` (atual) | 🟢 **Suporte Ativo** | 6 meses após `0.18.0-beta` ser marcada Latest |
| `0.16.2-beta` / `0.16.3-beta` | 🟡 Apenas Critical | 2026-12-01 |
| `< 0.16.2-beta` | 🔴 EOL (Sem patches) | Atualize Imediatamente |

---

## 5. Nossos Controles de Segurança (Release Engineering)

### 5.1 Chain of Trust SHA-256 em 3 fontes cruzadas

Cada release pública passa por validação:

```
(A) Build machine local (build_script.py + PowerShell Get-FileHash)
        └── Hash comparado com (B)
(B) Script stdlib-only generate_release_hashes.py (8 MB chunked FIPS 180-4)
        └── Hash comparado com (C)
(C) GitHub API GET /releases/assets/{id} → estado 'uploaded' + digest
        └── (A) == (B) == (C) → release liberada
```

### 5.2 Isolamento LGPL/GPL

- **Scapy 2.6.0 (GPL-2.0)** roda **estritamente em processo filho RPC separado** via Mere Aggregation.
- **Nenhum `import scapy` em código proprietário** (verificado por Ruff + testes automatizados).
- **pymodbus 3.6.9 (LGPL-3.0)** link dinâmico via `pip install -r requirements.txt` (relinkável pelo cliente).

### 5.3 Zero Telemetria, Zero Coleta de Dados

- App opera **100% OFFLINE**. Nenhum dado (MAC, IP, perfil de engenheiro, configuração de planta) sai do pendrive USB.
- Operação **B2B-only** (Pessoa Jurídica). Campos CPF / e-mail pessoal eliminados via técnica LGPD Art. 5º XII.

---

## 6. Informações PGP / Contato Direto CISO

```
CISO Responsável MCNA:  security@mirocontrol.com.br
Jurídico LGPD:          dpo@mirocontrol.com.br
```

---

> © 2026 Miro Control Automação Industrial LTDA. CNPJ [00.000.000/0001-00]
