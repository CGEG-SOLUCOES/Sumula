# PGE em Campo 2026 — Súmula Oficial da Partida

Formulário digital pra cada time entregar a Súmula Oficial via QR Code,
substituindo a entrega física. 100% gratuito: Firebase (já criado) +
GitHub Pages.

**Status:** as chaves do Firebase já estão coladas nos arquivos. As
regras de segurança já foram publicadas no console. Falta só subir pro
GitHub Pages.

---

## Arquivos deste pacote

| Arquivo               | O que é                                                          |
|-------------------------|---------------------------------------------------------------------|
| `sumula.html`          | Formulário que o Relator preenche (acessado via QR Code)             |
| `admin-sumulas.html`   | Painel da equipe de facilitação — mostra quem já entregou            |
| `gerador-qrcodes.html` | Gera os 20 QR Codes pra imprimir no Kit de Campo — **não sobe pro GitHub**, abre local no seu computador |
| `firestore.rules`      | Cópia do que já está publicado no console (só de referência)         |

---

## Passo 1 — Subir pro GitHub Pages

1. No repositório `EncontroServidores` (ou crie um novo, se preferir
   separar dos outros projetos), suba **`sumula.html`** e
   **`admin-sumulas.html`** pra raiz.
2. **Settings → Pages** → confirme branch `main`, pasta `/ (root)`.
3. Espera 1-2 minutos. O formulário fica em:
   `https://SEU-USUARIO.github.io/SEU-REPOSITORIO/sumula.html`
4. O painel de facilitação fica em:
   `.../admin-sumulas.html`

## Passo 2 — Trocar a senha do admin

Antes de publicar de verdade, abra `admin-sumulas.html`, procure
(Ctrl+F) por:

```js
const ADMIN_PASSWORD = "pge2026";
```

e troque `"pge2026"` pela senha que você quiser usar.

## Passo 3 — Gerar e imprimir os QR Codes

1. Abra `gerador-qrcodes.html` **no seu computador** (duplo clique
   abre no navegador — precisa de internet só pra carregar a lib de QR).
2. No campo do endereço, cole a URL real do seu site (a mesma do
   Passo 1, **sem** `sumula.html` no final — só até a barra `/`).
3. Clique em **"Gerar QR Codes"**.
4. Clique em **"🖨️ Imprimir"** (ou Ctrl+P) e imprima — cada card já
   sai com o nome do time e o QR certo.
5. Recorte e cole cada QR Code no Kit de Campo do time correspondente.

## Passo 4 — Testar antes do evento

1. Escaneia um dos QR Codes impressos (ou abre a URL direto no
   celular).
2. Confirma que o formulário já abre com o time certo selecionado.
3. Preenche um teste e envia.
4. Confirma no Firebase (console → Firestore Database → coleção
   `sumulas`) que o documento apareceu.
5. Escaneia o **mesmo QR Code de novo** → deve cair direto na tela
   "Súmula já recebida!" (a trava funciona por país, é permanente).
6. Abre `admin-sumulas.html`, digita a senha, confirma que o time de
   teste aparece como recebido, e que dá pra ver o conteúdo completo
   clicando na linha dele.
7. **Antes do evento valer**, apague esse documento de teste no
   console do Firestore (coleção `sumulas` → time testado → excluir).

---

## Como funciona a trava de "1 súmula por time"

Diferente da votação (que é anônima), aqui cada time tem identidade
clara — o país. O ID de cada documento no Firestore **é o próprio
país**, e as regras de segurança bloqueiam a criação de um segundo
documento com o mesmo ID. Isso significa que, mesmo que dois
integrantes do mesmo time escaneiem o QR e tentem enviar ao mesmo
tempo, só o primeiro envio é aceito — o segundo automaticamente cai na
tela de "já recebida".

Depois de enviada, a súmula é **permanente**: as regras não permitem
editar nem apagar (só você, manualmente, pelo console do Firebase, em
caso de erro).

## Avisos importantes

- Este app foi construído com apoio de IA. Teste o fluxo completo antes
  do dia do evento.
- A senha do admin protege só a *interface* de consulta — a trava de
  duplicidade de verdade está nas regras do Firestore, então é sólida.
- Plano gratuito do Firebase aguenta de sobra 20 times enviando súmulas
  (limite é de milhares de gravações por dia).
