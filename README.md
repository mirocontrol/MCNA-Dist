# 🚀 MC Network Analyzer — Releases Oficiais

> **Fonte oficial de downloads** do MC Network Analyzer.
> Distribuição **100% Standalone USB / Portable Offline / Zero Instalação** para engenheiros de campo.

---

## ⚠️⚠️⚠️ NÃO CLIQUE NO BOTÃO VERDE "Code" — BAIXE PELA ABA "Releases" ⚠️⚠️⚠️

| ❌ **ERRO MAIS COMUM** (você não baixou o executável) | ✅ **JEITO CERTO** (vem o EXE portable) |
|---|---|
| 🟩 Clicar no botão verde **Code → Download ZIP** na página inicial do repositório. | Ir até a **aba Releases** (menu lateral direito OU clique no botão abaixo) |
| Resultado: **10 KB** de arquivos markdown (README / LICENSE), sem EXE, sem runtime. | Resultado: **~42 MB** de ZIP portable com MC-NetworkAnalyzer.exe, runtime Python, launchers, etc. |

> **Por que o binário não está no git?** Repositórios git são para código-fonte / docs. Binários de 40 MB ficam armazenados apenas como **GitHub Release Assets** (storage otimizado, indexado por versão).

👇 **CLIQUE AQUI PARA BAIXAR A VERSÃO MAIS RECENTE:**

# [👉 📥 Ir para Releases / Downloads](https://github.com/mirocontrol/MCNA-Dist/releases/latest)

> Atalho: também clique no lado direito desta página, na área **"Releases"**, em **"Latest"**.

---

## 📦 Baixar a Última Versão (mesmo link acima, repetido por conveniência)

👉 **[github.com/mirocontrol/MCNA-Dist/releases/latest](https://github.com/mirocontrol/MCNA-Dist/releases/latest)**

Sempre baixe **todos os 3 anexos** da release (na área "Assets" da página da release):

| # | Arquivo | Tamanho | Finalidade |
|---|---------|---------|------------|
| 1 | `MC-NA-Portable_build*.zip` | ~42 MB | **Pacote cliente portable** (EXE + runtime + 4 launchers padrão + docs) |
| 2 | `hashes.txt` | < 1 KB | Hash **SHA-256 oficial** (formato GNU coreutils, com metadados) |
| 3 | `MC-NA-Portable_build*.zip.sha256` | 160 B | Side-file compatível com OpenSSL / `certutil -hashfile` / `sha256sum` |

---

## 🔐 Verificação de Integridade (ANTES de copiar para pendrive)

> **Executem ESTES PASSOS em um PC de ESCRITÓRIO (com antivírus atualizado), ANTES de conectar o pendrive USB na rede OT.** Evita man-in-the-middle e corrupção em transferência.

### Windows PowerShell (1 linha):

```powershell
Get-FileHash -Algorithm SHA256 .\MC-NA-Portable_build*.zip
```

Compare o valor no campo `Hash` com o hash da primeira linha de **`hashes.txt`**. Deve ser **idêntico** (case-insensitive, 64 caracteres hex).

### WSL / Git Bash / Linux (validação formal):

```bash
sha256sum -c hashes.txt
```

Saída **esperada**: `MC-NA-Portable_build2650705.zip: OK`

---

## ⚡ Requisitos Mínimos de Sistema

| Requisito | Valor |
|-----------|-------|
| **Sistema Operacional** | Windows 10 x64 22H2+ (build 19045+) **ou** Windows 11 x64 22H2+ (build 22621+) |
| **Arquitetura** | x86_64 / AMD64 (**não suporta ARM64 / Windows 7/8**) |
| **Memória RAM** | 4 GB mínimo | 8 GB recomendado |
| **Armazenamento** | 500 MB livres em disco ou pendrive USB (NTFS / ExFAT) |
| **Rede (obrigatória)** | Placa Ethernet Gigabit 1Gbps (Intel I210 / Realtek RTL8111) com **cabo CAT5e/CAT6 conectado** |
| **⚠️ WI-FI NÃO SERVE** | Profinet DCP L2 / LLDP / EtherNet/IP CIP **não trafegam em 802.11**. |
| **Driver L2 (opcional, recomendado)** | Npcap 1.79+ instalado **como Administrador** com opção "WinPcap API-compatible Mode" (para Profinet DCP / Ghost IP / LLDP). Sem Npcap = fallback automático GENERIC_LAN (L3 ARP+Ping). |
| **Privilégios** | Administrador (UAC) obrigatório para varredura L2 Profinet DCP. |

---

## 🧰 Protocolos Suportados (6 em Paralelo, 1 Clique)

| Camada | Protocolo | Uso Principal |
|--------|-----------|---------------|
| **L2 RAW** | **PROFInet DCP** (EtherType 0x8892) | Descoberta Siemens, Hirschmann, Phoenix Contact. Nome/IP/MAC/VLAN ID. |
| **L2 RAW** | **SNMP v1/v2c + LLDP** | Topologia física de switches (Vis.js GraphML export), OUI Lookup fabricante. |
| **L3/L4** | **EtherNet/IP CIP** | Rockwell Allen-Bradley, WEG CFW, Omron NX. Vendor ID / Device Type. |
| **L3/L4** | **S7comm Siemens** | S7-300/400/1200/1500. TSAP probing, SZL IDs, bloco de identificação. |
| **L4 TCP 502** | **Modbus TCP** | Tier-S Register Map Float32 4x endianness, KPIs P50/P95/P99. |
| **L4 RS-485** | **Modbus RTU Serial** | USB-RS485 (FTDI FT232R / CP210x). Baud 1200 ~ 921600, FC01/02/03/04/05/06/15/16. |
| **Fallback** | **GENERIC_LAN (ARP + Ping)** | Sem admin / sem Npcap. Nenhuma colisão na rede OT. |

---

## 🛠 Fluxo Rápido de Campo (60 segundos)

```
1.  Extraia TODO o ZIP para raiz do pendrive  (ex: D:\MC-NA-Portable_build2650705\)
2.  ❌ NUNCA execute diretamente de dentro do ZIP.
3.  [Plantas Siemens] Duplo-clique em:  00-Diagnostico-PreVarredura-Siemens.cmd
4.  [Todas as plantas] Duplo-clique em:  01-Iniciar-SomenteLeitura-Planta.cmd
5.  Aprova UAC (elev Admin) → Aguarde 10~15s.
6.  No app: Selecione placa Ethernet correta → "Iniciar Varredura" UMA VEZ.
7.  Aguarde 20~45s (6 protocolos).
8.  Salve Projeto (.mcna) → Exporte CSV → Exporte As-Built HTML.
9.  Grave nº Série do pendrive + horário no livro de visita ISO 27001 da planta.
```

---

## ⚠️ Hard Fail Safety — Planta Produtiva (CUMPRA SEMPRE)

| ✅ FAÇA | ❌ NÃO FAÇA |
|---------|-------------|
| Sempre use `01-Iniciar-SomenteLeitura-Planta.cmd` para 99% dos casos | Não execute `03-Iniciar-ModoEscrita-Bancada.cmd` sem Ordem de Serviço formal + assinatura digital |
| Sempre rode `00-Diagnostico-PreVarredura-Siemens.cmd` antes em plantas Siemens | Não use Wi-Fi para conectar em redes OT (Profinet DCP / LLDP não passa) |
| Sempre valide SHA-256 **ANTES** de copiar para o pendrive | Não renomeie arquivos `.exe` ou `.cmd` da raiz de distribuição |
| Extraia TODO o ZIP para pasta local primeiro | Não compartilhe o pendrive com outras ferramentas de captura de rede |

---

## 🌐 Idiomas Suportados (100% Paridade I18N)

| Idioma | Código | Localização |
|--------|--------|-------------|
| 🇧🇷 Português Brasil | `pt-BR` | Padrão (fallback) |
| 🇺🇸 English (US) | `en-US` | UI completa |
| 🇪🇸 Español | `es-ES` | UI completa |

Troca de idioma em tempo real: Menu superior direito → 🌍 Seletor de bandeira.

---

## 📚 Atribuições de Bibliotecas (Licenças)

Lista **completa e assinada** dentro do pacote ZIP em: `distribution_assets/ATRIBUICOES_BIBLIOTECAS.md`

Resumo rápido:

| Biblioteca | Licença | Observação Jurídica |
|------------|---------|---------------------|
| pymodbus 3.6.9 | LGPL-3.0 | Link dinâmico relinkável via pip local |
| scapy 2.6.0 | GPL-2.0 | **EXECUTA ESTRITAMENTE EM PROCESSO FILHO RPC SEPARADO (Mere Aggregation)**. Nenhum `import scapy` no código proprietário. |
| FastAPI 0.115 / Pydantic v2 / Uvicorn | MIT | Backend loopback-only |
| NumPy 2.x / Pandas 2.2 / ReportLab 4.2 | BSD-3 | KPIs e As-Built PDF |
| Vis.js Network 9.x / Tailwind CSS 3 / Solid.js | MIT | Topologia + UI reativa |
| CPython 3.11.9 stdlib | PSF 2.0 | Runtime embed portable |

---

## 🛡 Modo Padrão: SOMENTE LEITURA

O app **inicia bloqueado** para 100% das operações de escrita em rede. O modo escrita **exige**:

1. Launcher exclusivo: `03-Iniciar-ModoEscrita-Bancada.cmd`
2. Checkout Session exclusiva (1 sessão ativa por vez)
3. **Código de desbloqueio de 6 dígitos** (engenharia)
4. **Dupla confirmação** por popup em cada operação de escrita
5. **Audit Chain IMUTÁVEL** (todas escritas logadas com SHA-256 encadeado)

Cumpre **IEC 62443-4-2** em operação padrão Read-Only.

---

## 📞 Suporte Técnico Corporativo

| Canal | Contato |
|-------|---------|
| **Identidade Empresarial** | 📋 Miro Control Automação Industrial LTDA · CNPJ 64.335.954/0001-38 |
| WhatsApp Campo 24/7 | +55 43 98830-8437 |
| E-mail Engenharia / Suporte | network_analyzer@mirocontrol.com.br |
| Site Oficial | [https://mirocontrol.com.br](https://mirocontrol.com.br) |
| Issues Bugs (repo público) | [github.com/mirocontrol/MCNA-Dist/issues](https://github.com/mirocontrol/MCNA-Dist/issues) |

---

> **MC Network Analyzer** · (c) 2026 **Miro Control Automação Industrial LTDA.** · CNPJ 64.335.954/0001-38  
> Todo o conteúdo deste repositório é distribuído sob a **EULA comercial MCNA** (ver `LICENSE.txt`).  
> Código-fonte do app permanece em repositório privado (`mirocontrol/MC-NetworkAnalyzer`). Apenas binários de release e documentação de campo são publicados aqui.
