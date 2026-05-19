# RPG Chain

Sistema de ficha de RPG para **Vampiro: A Máscara — Idade das Trevas**, com integridade verificável via blockchain.

Sem servidor, sem conta, sem mensalidade. Cada um dono dos próprios dados.

---

## 🎲 Acesso

# RPG Chain

## Links

- [Ficha Principal](https://armanfm.github.io/ficha-json/ficha.html)

- [Ficha V2](https://armanfm.github.io/ficha-json/ficha2.html)

- [Painel Admin](https://armanfm.github.io/ficha-json/admin.html)

- [Painel Narrador](https://armanfm.github.io/ficha-json/painel_admin.html)

- [Tracker Online](https://armanfm.github.io/ficha-json/vtm_tracker_online.html)

- [Ficha PDF Viewer](https://armanfm.github.io/ficha-json/ficha_vtm_com_pdf.html)

- [Controle](https://armanfm.github.io/ficha-json/controle.html)

- [Controle](https://armanfm.github.io/ficha-json/controle2.html)

- [D&D 5e](https://armanfm.github.io/ficha-json/dnd5e.html)
  

---

## O que cada um faz

### Ficha do Jogador

A ficha completa de Vampiro, totalmente editável e offline-first. Tudo fica salvo no navegador. Nada vai pra servidor nenhum.

**O que tem:**

- **Ficha** — atributos (7/5/3), perícias (13/9/5), virtudes, força de vontade, humanidade, sangue
- **Disciplinas** — adicione disciplinas e poderes customizados, com pool de dados (atributo + habilidade + dificuldade), custo de sangue, custo de FdV, duração e descrição
- **Antecedentes** — os 10 clássicos editáveis (Aliados, Contatos, Domínio, Fama, Gregário, Influência, Mentor, Recursos, Séquito, Status)
- **Qualidades & Defeitos** — com cálculo automático de pontos bônus
- **Anotações** — privadas, ficam só no dispositivo, não vão no JSON
- **Combate** — modo de jogo com sangue, vitalidade, força de vontade temporária, boosts de sangue, efeitos ativos com duração, log da sessão, integração com Discord

**Botões no header:**

- 💾 **Salvar** — força o save imediato
- ⬇ **Exportar JSON** — baixa a ficha completa em JSON para mandar ao mestre
- ⬆ **Carregar JSON** — carrega uma ficha salva
- 🎮 **Discord** — configura webhook para enviar rolagens automáticas pro canal da crônica

**Integridade da Ficha (aba Combate):**

Calcula o hash SHA-256 canônico da ficha (estável, sem timestamps). O mestre aprova esse hash no painel dele. Se a ficha for alterada depois da aprovação, o hash não bate mais e o sistema avisa: **✕ ADULTERADA**.

**Discord automático no combate:**

Ao rolar um poder, manda direto pro canal do Discord um embed completo:

```
🩸 Lucien — Fortitude
Disciplina Toreador · Nível 2

Pool: 6 dados (Dif. 6)
Resultado: 3 sucessos
Dados: 7, 8, 3, 5, 9, 2

Hash: ab991f2e3d5a8c4f… · 19:42:15
```

Dados que passaram da dificuldade vêm em negrito, botches riscados. O hash é determinístico — o mestre vê na hora, sem ninguém precisar copiar nada.

---

### Painel do Narrador

Gerenciador de fichas e crônica. Recebe os JSONs dos jogadores, valida com Gemini, aprova hashes, opcionalmente registra na blockchain Sepolia.

**O que tem:**

- **Crônica** — cria a crônica, adiciona slots para cada jogador
- **Jogadores** — seleciona um slot, faz upload do JSON do jogador, vê a ficha renderizada read-only, analisa com Gemini, aprova ou rejeita
- **Onchain** — conecta MetaMask na Sepolia, minta o NFT da crônica, aprova hashes na blockchain, verifica fichas publicamente

**Análise por Gemini:**

Cada narrador usa sua própria chave (configurada localmente, nunca sai do dispositivo).

- **Primeira ficha** — valida regras de criação: atributos 7/5/3, perícias 13/9/5, disciplinas de clã, virtudes, pontos bônus, sangue por geração
- **Evoluções** — compara JSON anterior vs novo, detecta o que mudou, calcula o XP gasto pela tabela oficial, verifica saldo. Se algum ponto diminuiu sem motivo, bloqueia antes mesmo de chamar a IA

**Onchain (opcional):**

Para quem quiser registro permanente e verificável:

- O mestre minta um NFT que representa sua autoridade narrativa da crônica
- Cada hash aprovado é gravado na Sepolia vinculado a esse NFT
- Qualquer um pode verificar publicamente se uma ficha foi aprovada por aquela crônica
- Custa só gas — barato na mainnet, zero na Sepolia

---

## Filosofia

| Camada | O que faz | Onde fica |
|---|---|---|
| **PouchDB / localStorage** | Guarda tudo no dispositivo de cada um | Local |
| **Gemini** | Valida regras, calcula XP, audita evolução | API key própria |
| **Discord webhook** | Envio automático de rolagens | Webhook do canal |
| **Blockchain (opcional)** | Prova imutável do estado aprovado | Sepolia |

Nenhuma camada depende das outras. Se o Gemini cair, o jogo continua. Se a blockchain cair, a ficha continua. Se o Discord cair, dá pra digitar manualmente. Cada uma resolve o que é boa em resolver.

---

## Fluxo completo

```
1. Jogador preenche a ficha
2. Exporta o JSON
3. Manda pro narrador (WhatsApp, Discord, o que for)
4. Narrador faz upload no painel
5. Gemini valida automaticamente
6. Narrador aprova ou rejeita
7. Hash salvo localmente
8. (Opcional) Hash gravado na Sepolia

Durante a sessão:
- Jogador rola poderes → resultado vai pro Discord automático
- Mestre dá XP de boca, jogador anota direto na ficha clicando nas bolinhas
- Quando o personagem evolui, exporta o JSON de novo e manda pro narrador
- Gemini detecta o que mudou e calcula o custo
```

---

## Stack

- HTML puro, sem framework, sem bundler
- PouchDB para armazenamento local
- Web Crypto API para SHA-256
- Discord Webhook API para rolagens
- Google Gemini API para validação
- ethers.js v6 + MetaMask para Sepolia
- Solidity 0.8.20 — contrato sem dependências externas

---

## O que o sistema **não** faz

- Não hospeda nada em servidor
- Não tem login ou conta
- Não vende NFT de personagem
- Não bloqueia o roleplay
- Não substitui o narrador — apenas economiza tempo dele
