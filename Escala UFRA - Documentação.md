# Escala UFRA

**Universidade Federal Rural da Amazônia — Campus Belém**
**Controle de Frequência e Escala de Trabalho**

---

## Sobre

Aplicação web interativa para controle de frequência mensal de servidores da UFRA. Gera tabela mensal com cálculos automáticos de horas trabalhadas, carga esperada, saldo, horas extras e atrasos, considerando fins de semana e feriados oficiais.

---

## 📌 Entendendo o Calendário UFRA

### Hierarquia do Calendário

O calendário utilizado nesta aplicação segue a seguinte hierarquia:

```
Governo Federal (MGI) — base oficial
  └── Feriados nacionais (Lei Federal)
  └── Pontos facultativos nacionais
       └── Estado do Pará — feriados estaduais
            └── Município de Belém — feriados municipais
                 └── UFRA — calendário administrativo próprio
```

### Como funciona na prática

1. **Governo Federal** publica anualmente uma portaria do MGI (Ministério da Gestão e Inovação) com os feriados e pontos facultativos do ano
2. **A UFRA** adota o calendário do Governo Federal como base e pode:
   - Adicionar **recessos administrativos** (ex: final de ano, carnaval)
   - Definir **pontos facultativos próprios** (ex: aniversário da UFRA)
   - Antecipar ou postergar feriados que caem em fins de semana
3. **Cada ano** a UFRA lança uma portaria/resolução com o calendário oficial

> ⚠️ **Importante:** Os feriados nacionais fixos (como 7 de setembro, 12 de outubro, etc.) e os feriados móveis (Carnaval, Páscoa, Corpus Christi) são calculados **automaticamente** para qualquer ano. O que precisa ser verificado **todo ano** são os pontos facultativos específicos e recessos da UFRA.

---

## 📆 Como Atualizar para o Ano Seguinte

### Passo a passo anual

```javascript
// No arquivo escala_ufra.html, localize a seção CONFIGURAÇÃO:

const ANO_ATUALIZACAO = 2026;  // ← ALTERE AQUI para o novo ano

// E na seção CALENDARIO_UFRA_EXTRAS, adicione o novo bloco:

const CALENDARIO_UFRA_EXTRAS = {
  2026: [ /* datas específicas UFRA 2026 */ ],
  2027: [ /* ← ADICIONE AQUI as datas específicas UFRA 2027 */ ],
};
```

1. **Consulte** o calendário oficial da UFRA para o novo ano (site: ufra.edu.br)
2. **Consulte** a portaria do MGI com os feriados e pontos facultativos do Governo Federal
3. **Compare** com o calendário do ano anterior — identifique diferenças
4. **Atualize** `ANO_ATUALIZACAO` no código para o novo ano
5. **Adicione** as datas específicas da UFRA em `CALENDARIO_UFRA_EXTRAS`
6. **Teste** gerando a tabela de cada mês para verificar se os feriados estão corretos

### O que é automático (não precisa atualizar)

| Item | Cálculo |
|---|---|
| Feriados nacionais fixos | Automático (Lei Federal) |
| Feriados estaduais (PA) | Automático |
| Feriados municipais (Belém) | Automático |
| Carnaval, Sexta-feira Santa | Automático (algoritmo da Páscoa) |
| Corpus Christi | Automático (algoritmo da Páscoa) |
| Fins de semana | Automático |

### O que precisa ser verificado todo ano

| Item | Onde atualizar |
|---|---|
| Pontos facultativos que mudam de data | `CALENDARIO_UFRA_EXTRAS[ano]` |
| Recesso administrativo da UFRA | `CALENDARIO_UFRA_EXTRAS[ano]` |
| Datas comemorativas específicas da UFRA | `CALENDARIO_UFRA_EXTRAS[ano]` |
| Feriados que caem em FDS (se a UFRA antecipar/postergar) | `CALENDARIO_UFRA_EXTRAS[ano]` |
| `ANO_ATUALIZACAO` | Constante no início do script |

---

## Arquivos

| Arquivo | Descrição |
|---|---|
| `escala_ufra.html` | ✅ Aplicação principal (versão atual) |
| `Escala UFRA - Documentação.md` | ✅ Documentação do projeto (este arquivo) |
| `Planilha de Escala de Trabalho - UFRA.md` | Versão anterior da documentação com código embutido |
| `planilha_escala_trabalho (1).html` | Versão simplificada anterior (mantida como backup) |

---

## Funcionalidades

### ✅ Gerais
- Geração automática da tabela do mês selecionado
- Carga horária diária configurável (4h, 6h, 8h, 9h, 10h)
- Intervalo de almoço configurável (0 a 2h)
- Horários de entrada/saída padrão configuráveis
- Cálculo automático de horas trabalhadas, diferença e saldo
- Destaque visual para horas extras (verde) e atrasos (vermelho)

### ✅ Calendário UFRA Completo (qualquer ano)
- Feriados nacionais fixos (Lei Federal)
- Feriados estaduais do Pará
- Feriados municipais de Belém
- Feriados móveis calculados dinamicamente (Carnaval, Páscoa, Corpus Christi, Sexta-feira Santa)
- Pontos facultativos nacionais e específicos da UFRA
- Recessos administrativos da UFRA (configuráveis por ano)
- Identificação automática de fins de semana e feriados (campos desabilitados)
- **Sistema de calendário anual**: configure apenas o que muda a cada ano

### ✅ Controle por Modalidade
- Presencial 🏢 (fundo verde)
- Remoto 🏠 (fundo azul)
- Híbrido 🔄 (fundo amarelo)
- Contador de horas em tempo real por modalidade
- Totais detalhados no resumo (dias + horas por modalidade)

### ✅ Validação de Horários
- Detecta quando horário de saída é anterior ou igual ao de entrada
- Indicador visual vermelho nos campos inválidos
- Ícone de alerta (!) na coluna de validação
- Dias com erro são contabilizados separadamente no resumo
- Tooltip explicativo ao passar o mouse no ícone

### ✅ Persistência
- Salvamento automático no navegador (localStorage)
- Exportar para CSV com cabeçalho informativo e resumo completo
- Salvar planilha como arquivo JSON (com versão e data)
- Carregar planilha a partir de arquivo JSON

### ✅ Navegação
- Navegação entre meses com botões ◀ ▶
- Atalhos de teclado:
  - `Ctrl+G` — Gerar tabela
  - `Ctrl+E` — Exportar CSV
  - `Ctrl+S` — Salvar planilha (JSON)
  - `Ctrl+O` — Abrir/carregar planilha
  - `Ctrl+L` — Limpar dados

### ✅ Interface
- Design responsivo (desktop e mobile)
- Cards de resumo com totais, médias e distribuição por modalidade
- Toast notifications para feedback de ações
- Tooltips informativos em todos os botões e elementos interativos

---

## Melhorias Implementadas (v2.0)

Em relação às versões anteriores, foram implementadas:

### 1. Sistema de Calendário Anual
- **Antes**: Feriados hardcoded apenas para 2026 — exigia edição manual de todo o código
- **Agora**: Constante `ANO_ATUALIZACAO` e objeto `CALENDARIO_UFRA_EXTRAS` por ano
- Documentação clara no código sobre como atualizar
- Feriados móveis calculados dinamicamente via algoritmo de Gauss
- Intervalo de anos ampliado (5 anos para trás e para frente)

### 2. Validação de Horários
- **Antes**: Nenhuma validação
- **Agora**: Detecção de saída anterior/igual à entrada com destaque visual

### 3. Navegação entre Meses
- **Antes**: Apenas select mês/ano
- **Agora**: Botões ◀ ▶ com indicador visual do mês atual

### 4. Renomeação e Organização
- `planilha_escala_trabalho (1).html` → `escala_ufra.html`
- Código e documentação separados em arquivos distintos

### 5. Feedback Visual Aprimorado
- Toast notifications no lugar de alertas nativos
- Ícone de validação (✓/!) por linha na tabela

### 6. Chave de Storage Padronizada
- `planilhaEscala` → `escalaUFRA`

### 7. Indicador de Versão
- Campo `versao: '2.0'` no JSON exportado

---

## Como Usar

1. Abra o arquivo `escala_ufra.html` em qualquer navegador moderno
2. Selecione o mês e ano desejados
3. Configure carga horária, intervalo de almoço e horários padrão
4. Clique em **Gerar** (ou `Ctrl+G`)
5. Ajuste horários de entrada/saída por dia conforme necessário
6. Selecione a modalidade de trabalho (Presencial/Remoto/Híbrido)
7. Acompanhe os totais nos cards de resumo
8. Exporte para CSV ou salve como JSON ao final

---

## Estrutura do Código (seções principais)

```
scssCopiarCONFIGURAÇÃO
├── ANO_ATUALIZACAO          ← atualize aqui todo ano
├── FERIADOS_NACIONAIS       ← fixos (não mexer)
├── FERIADOS_ESTADUAIS       ← fixos (não mexer)
├── FERIADOS_MUNICIPAIS      ← fixos (não mexer)
├── PONTOS_FACULTATIVOS      ← fixos (não mexer)
└── CALENDARIO_UFRA_EXTRAS   ← adicione bloco para cada ano

CÁLCULO DE FERIADOS MÓVEIS   ← automático (não mexer)
INICIALIZAÇÃO                ← selects, navegação
VALIDAÇÃO                   ← horários entrada/saída
CÁLCULOS                    ← horas, diferenças
PERSISTÊNCIA                ← localStorage + JSON
GERAR TABELA                ← montagem da tabela HTML
FERIADOS DO MÊS             ← exibição dinâmica
RESUMO                      ← cards de totais
EXPORTAR CSV                ← geração de relatório
ATALHOS DE TECLADO          ← Ctrl+G/E/S/O/L
```

---

## Tecnologias

- HTML5
- CSS3 (Flexbox, Grid, Gradientes, Animações, Design Responsivo)
- JavaScript (Vanilla, ES6+)
- localStorage para persistência
- Algoritmo de Gauss para cálculo da Páscoa (feriados móveis)

---

## Desenvolvido por

**Ewerton Uchôa** — 2026
