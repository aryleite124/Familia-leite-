# Carnê da Família Leite

App de controle das contas do mês, plano de gastos e metas de reserva.
Página estática, sem servidor e sem banco de dados: os dados ficam salvos no próprio aparelho.

## Arquivos

| Arquivo | Para que serve |
|---|---|
| `index.html` | O app inteiro: tela, lógica e estilo |
| `manifest.webmanifest` | Faz o app instalar na tela de início com nome e ícone |
| `sw.js` | Cache offline — o app abre mesmo sem internet |
| `icone-*.png` | Ícones do app |

## Publicar no GitHub Pages

1. Crie um repositório **público** em github.com — sugestão de nome: `carne-familia-leite`.
2. Na tela do repositório vazio, clique em **uploading an existing file** e arraste os arquivos desta pasta. Depois **Commit changes**.
3. Vá em **Settings → Pages**.
4. Em *Source*, escolha **Deploy from a branch**. Em *Branch*, escolha **main** e a pasta **/ (root)**. Salve.
5. Espere 1 a 2 minutos e recarregue a página. O endereço aparece no topo:
   `https://SEU-USUARIO.github.io/carne-familia-leite/`

O repositório precisa ser público para o Pages funcionar no plano gratuito. Como não há
dados pessoais dentro do código — tudo o que você digita fica só no seu celular — isso não
expõe nada das suas finanças.

## Instalar no celular

**iPhone (Safari):** abra o endereço, toque no botão de compartilhar e escolha
*Adicionar à Tela de Início*.

**Android (Chrome):** abra o endereço, menu de três pontos, *Instalar aplicativo*.

Instalar não é só estética: o navegador passa a tratar os dados do app como permanentes,
em vez de cache descartável. Faz diferença real para não perder o histórico.

## Onde os dados ficam

No armazenamento local do navegador, naquele aparelho. Não sobem para lugar nenhum.

Consequências:
- Cada aparelho tem seus próprios dados. Celular e computador não conversam entre si.
- Limpar os dados do navegador apaga tudo.

O app avisa quando passa de 30 dias sem backup. Uma vez por mês, ao fechar as contas, abra **Plano → Backup dos dados →
Baixar arquivo de backup** e guarde o arquivo `.json` no Drive, no e-mail ou onde preferir.
Para voltar tudo em outro aparelho, é só carregar esse arquivo na mesma tela.

## Contas fixas e vigência

Uma conta marcada como fixa entra sozinha em **todos os meses seguintes**, sem precisar
lançar de novo. Cada uma tem uma vigência:

- **Começa em** — a partir de qual mês ela vale. Meses anteriores ficam intactos.
- **Repetir até** — deixe vazio para não ter fim; preencha em financiamentos e cursos
  com número fechado de parcelas. Depois desse mês ela para de aparecer.

A aba **Fixas** mostra os próximos 6 meses com o total já reservado, para conferir de
relance o que vai cair.

Cada lançamento automático recebe um identificador previsível (`fixa@mês`), então os dois
celulares geram exatamente o mesmo registro e a mesclagem nunca duplica a conta.

## Módulos

**Contas** — o carnê do mês. **Plano** — orçamento e divisão da renda.
**Metas** — reserva e objetivos. **Painel** — KPIs, gráficos e plano de evolução.
**Mais** — Contas fixas, Investimentos, Conquistas, Sincronização, Backup e Configurações.

### Investimentos

Cadastre cada aplicação e registre duas coisas ao longo do tempo:

- **Aporte** — quanto você colocou. Sobe o saldo e o total aportado.
- **Atualizar saldo** — o número que aparece no app do banco. A diferença entre saldo e
  aportado é o rendimento acumulado.

O app não conecta em banco nem corretora. O trabalho dele é guardar o histórico, calcular
o rendimento e desenhar a curva do patrimônio.

### Configurações

Nome de quem usa o aparelho, tema (automático, claro ou escuro) e a zona de risco com
três limpezas: só o mês exibido, o cache de imagens, ou tudo. Com sincronização ligada,
apagar tudo limpa o repositório e atinge o outro aparelho — por isso o botão de backup
vem antes.

## Detalhes de uso

- **Tema escuro** entra sozinho conforme o ajuste do celular.
- **Arraste para o lado** na tela de contas ou plano para trocar de mês.
- **Excluir pede confirmação**: o botão muda e você toca de novo.

## Publicar uma versão nova

Suba o `index.html` alterado e **troque o número em `VERSAO` no `sw.js`**
(`carne-v1` → `carne-v2`). Sem isso, o cache antigo continua sendo servido e a atualização
não aparece.

## Sincronizar entre aparelhos

O app usa um segundo repositório, **privado**, como banco de dados. Gratuito, sem prazo
de validade e sem servidor para manter no ar. Cada alteração vira um commit, então você
ganha histórico completo de brinde: dá para voltar a qualquer versão anterior pelo site
do GitHub.

**Preparar (uma vez):**

1. Crie um repositório **privado** chamado `carne-dados` e marque **Add a README file**
   (sem isso a branch `main` não existe e a primeira gravação falha).
2. GitHub → Settings → Developer settings → Personal access tokens →
   **Fine-grained tokens** → *Generate new token*.
3. Em **Repository access**, escolha *Only select repositories* e marque só o `carne-dados`.
4. Em **Permissions → Repository permissions → Contents**, marque **Read and write**.
5. Em *Expiration*, escolha um prazo longo ou *No expiration*.
6. Gere e copie o token.

**Ligar em cada aparelho:** abra o app → aba **Plano** → *Ativar sincronização* → cole
`seu-usuario/carne-dados` e o token.

**Como funciona:** ao abrir, o app compara a data do que está no aparelho com a do
repositório e fica com a versão mais recente. Ao alterar algo, envia sozinho poucos
segundos depois. Sem internet, continua funcionando normalmente e sobe quando voltar
o sinal.

**Regra única:** não lance contas em dois aparelhos ao mesmo tempo sem abrir um depois
do outro. Quem salvou por último vence, e o outro perde o que digitou.

**Segurança:** o token fica guardado só no aparelho, nunca entra no backup e não faz
parte do código publicado. Perdeu o celular? Revogue o token no GitHub e o acesso morre
na hora, sem mexer nos dados.

## Comprovantes

O botão **Pagar** não marca a conta como paga: ele abre a câmera. A conta só é quitada
quando a foto do comprovante entra. É o que transforma o app em prestação de contas
de verdade entre duas pessoas.

- A foto é reduzida para no máximo 1400px e comprimida antes de subir — cada comprovante
  fica em torno de 200 KB.
- Fica guardada em `comprovantes/` no repositório privado e em cache no aparelho.
- Sem internet, a conta é quitada normalmente e a foto sobe quando o sinal voltar.
- PDF do banco também serve: ao tocar em Pagar, escolha *Escolher arquivo ou PDF*.

**Exceção:** débito automático e afins não têm recibo. Abra a conta e use
*Marcar paga sem comprovante* — ela fica com um aviso âmbar, visível para os dois.

## Duas pessoas usando

Instale nos dois aparelhos com o mesmo repositório e token, e preencha **Quem usa este
aparelho** na tela de sincronização. Cada pagamento passa a registrar quem fez.

Antes de gravar, o app baixa a versão do repositório e **mescla** com a local, conta por
conta: quem alterou por último vence naquela conta específica, não no arquivo inteiro.
Se você quitar a Coelba enquanto sua esposa lança o mercado, as duas coisas sobrevivem.
Exclusões também são respeitadas — o que um apagou não volta pelo outro.

## Trocar de aparelho

Instale o app no celular novo, ative a sincronização com o mesmo repositório e token.
Tudo aparece. O aparelho velho pode ser desconectado em Plano → engrenagem →
*Desconectar este aparelho*.
