# Argos — Armazém
### Projeto 02 | Power Developers | Kodie Academy
**Desafio:** Visualização e Alocação Inteligente de Carga no Autoportante — Wilson Sons

---

## 🔗 Acesso

- **Aplicação (Lovable):** https://argos-yard-vision.lovable.app

  
- **Planilha de dados (Google Sheets):** https://docs.google.com/spreadsheets/d/e/2PACX-1vTrKkYZfTk7vTs7hqRq8XtF34DNMGf1hXHA_lAsNbvqq51GhSUM5UwolxId8MXcYPloyeYgqTFkFUQN/pub?output=xlsx
  
  
- **Automação (Make)** https://us2.make.com/public/shared-scenario/PiWbLtTU3Sa/integration-google-sheets

> A aplicação é somente leitura no momento (visualização em tempo real). A criação/edição de contêineres e posições é feita diretamente na planilha, conforme detalhado na seção "Como usar" abaixo.

---

## 📋 Descrição do problema

No armazém autoportante da Wilson Sons, o sistema TECSYS gerencia as alocações, mas o processo de descarregamento ainda é feito manualmente, sem visualização em tempo real. O operador de empilhadeira decide e informa manualmente onde cada carga será armazenada, com base em experiência própria e visão parcial do pátio — o que pode gerar deslocamentos desnecessários, maior tempo de operação e risco de erro humano, especialmente no tratamento de cargas com restrições de segurança (IMO).

## 💡 Solução desenvolvida

O **Argos — Armazém** é um gêmeo digital do pátio que automatiza a sugestão de posição de armazenamento para cada contêiner e exibe o estado do armazém em tempo real, reduzindo a decisão manual do operador a uma simples conferência visual.

A solução integra 3 ferramentas:

1. **Google Sheets** — base de dados central: contêineres a alocar, mapa de posições do pátio, regras de decisão e histórico de resultados.
2. **Make + Google Gemini** — motor de automação e decisão: lê os contêineres pendentes, aplica um conjunto de regras de negócio (segurança, ocupação, esforço e distância) e grava a posição sugerida com uma justificativa em linguagem natural.
3. **Lovable** — painel visual (gêmeo digital) que exibe o pátio em grid, colorido por status, com histórico das últimas alocações e a justificativa de cada uma.

### Lógica de decisão aplicada pela IA

**Regras de eliminação (aplicadas antes de qualquer pontuação):**
- Carga perigosa (IMO) só pode ser alocada em posição de zona segura.
- A posição precisa suportar o peso do contêiner e estar livre.
- Se a taxa de ocupação geral do armazém estiver abaixo de 40%, apenas posições de nível 1 a 4 (das 7 disponíveis) são consideradas — reduzindo o deslocamento vertical da lança quando o pátio está pouco ocupado.

**Critérios de pontuação (para as posições elegíveis):**
- **Esforço:** posições de nível mais baixo pontuam mais (menos movimento da lança).
- **Distância:** posições em filas mais próximas da entrada pontuam mais.
- **Bônus de peso:** contêineres mais pesados em níveis baixos recebem pontuação extra de estabilidade.
- **Bônus de rota:** agrupar contêineres da mesma rota de destino no mesmo bloco recebe pontuação extra, otimizando deslocamentos futuros.

A posição de maior pontuação é escolhida, e a IA retorna uma justificativa explicando os critérios aplicados — garantindo que a decisão seja sempre rastreável e explicável para o operador.

## ⚙️ Principais funcionalidades

- Alocação automática de contêineres pendentes, sem intervenção manual do operador.
- Aplicação de regra de segurança obrigatória para cargas perigosas (IMO).
- Aplicação de regra de ocupação, restringindo o uso de níveis altos quando o armazém está pouco ocupado.
- Justificativa em linguagem natural para cada alocação, gerada pela IA.
- Visualização em tempo real do pátio em formato de grid, organizado por bloco e nível, com código de cores (livre / ocupado / zona segura).
- Painel com o histórico das últimas alocações e seus motivos.
- Indicadores gerais do armazém: total de posições, ocupadas, livres e zonas seguras.
- Botão de atualização manual dos dados no painel.

## 🛠️ Tecnologias utilizadas

- **Google Sheets** — armazenamento de dados (contêineres, mapa do pátio, regras, resultados)
- **Make** — automação do fluxo (leitura, chamada de IA, gravação de resultados)
- **Google Gemini** — motor de decisão e geração de justificativa
- **Lovable** — interface visual (gêmeo digital do armazém)

## 📖 Como usar

1. Um novo contêiner é adicionado na aba `CONTEINERES` da planilha, com status `PENDENTE`.
2. O Make identifica o contêiner pendente, consulta as posições livres do pátio e a taxa de ocupação geral, e envia esses dados para o Gemini.
3. O Gemini aplica as regras de negócio e retorna a posição sugerida com justificativa.
4. O Make grava o resultado na aba `RESULTADOS`, atualiza o status do contêiner para `ALOCADO` e marca a posição do pátio como ocupada.
5. O painel do Lovable exibe a atualização assim que os dados são atualizados (botão "Atualizar dados").

## 🚧 Próximos passos (evolução futura)

- Criação, edição e exclusão de contêineres e posições diretamente pela interface do Lovable (sem necessidade de acessar a planilha), via automações adicionais no Make.
- Refinamento do peso do critério de estabilidade (peso do contêiner) na pontuação.
- Redução do intervalo de atualização automática para uma experiência ainda mais próxima do tempo real.

---

**Autor:** Ewerton
**Curso:** Kodie Academy — Turma Power Developers
