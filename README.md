# MeuNeg-cio
Desafio DIO para um negócio on-line

# A dor escolhida e por que ela vale um negócio
A dor: o freelancer angolano que tenta vender serviços ao estrangeiro tem talento, mas não tem “operação”. Isso aparece de três formas:

Perde clientes por falta de seguimento. Contactos espalhados pelo WhatsApp, LinkedIn e plataformas, sem registo de quem respondeu e de quando voltar a falar.
Não sabe quanto ganha de facto. Entre a taxa da plataforma, o método de pagamento e a conversão para kwanzas, o rendimento líquido é uma incógnita.
Parece amador. Não tem propostas, faturas e portfólio padronizados, o que pesa muito perante clientes estrangeiros.

# BMC (Business Model Canvas)
## Business Model Canvas

| Bloco | Conteúdo |
| --- | --- |
| **Segmentos de clientes** | Freelancers angolanos de 18 a 34 anos, iniciantes e intermédios, em serviços digitais |
| **Proposta de valor** | "Organiza os teus clientes, cobra como profissional e vê quanto ganhas em kwanzas" |
| **Canais** | LinkedIn, grupos de Facebook e WhatsApp, perfil público (/p/username), parcerias com formadores e universidades |
| **Relação com clientes** | Self-service, comunidade em WhatsApp, modelos de mensagens e conteúdo educativo |
| **Fontes de receita** | Freemium; plano pago (hipótese: US$5/mês); cobrança por Multicaixa Express ou referência |
| **Recursos-chave** | App (Lovable + Supabase), modelos de mensagens PT/EN, conhecimento de pagamentos em Angola, comunidade |
| **Atividades-chave** | Construir e iterar o MVP, entrevistar utilizadores, produzir conteúdo, manter o guia de pagamentos |
| **Parcerias-chave** | Formadores e escolas, comunidades de freelancers, plataformas de pagamento e fintechs locais |
| **Estrutura de custos** | Lovable e Supabase, domínio, marketing, taxas de cobrança da subscrição, tempo do fundador |

# A tese
Freelancers angolanos que vendem para o exterior vão usar uma ferramenta que organiza clientes, faturas e rendimento em kwanzas, e uma parte deles pagará por ela.

# Prompt
*PAPEL*
Atua como um engenheiro full-stack sénior e designer de produto. Vais construir uma aplicação web chamada "FreelaAO", um painel de gestão para freelancers angolanos que prestam serviços a clientes no estrangeiro.

CONTEXTO E PROBLEMA
O utilizador é um freelancer em Angola (tradutor, editor de vídeo, designer, assistente virtual, etc.) que recebe pagamentos em USD ou EUR e gasta em kwanzas (AOA). Hoje gere tudo no WhatsApp e em folhas soltas: perde clientes por falta de seguimento, não sabe quanto ganhou no mês e não sabe quanto lhe fica depois das taxas e do câmbio.

UTILIZADOR-ALVO
Freelancer a trabalhar sozinho, em telemóvel e computador, com internet por vezes lenta. A app deve ser leve, rápida e utilizável no telemóvel (mobile-first).

IDIOMA
Toda a interface em português de Angola. Datas no formato dd/mm/aaaa. Moedas: AOA, USD, EUR.

FUNCIONALIDADES DO MVP (por ordem de prioridade)

1. Autenticação
   - Registo e login com email e palavra-passe (Supabase Auth).
   - Cada utilizador só vê os seus próprios dados (Row Level Security em todas as tabelas).

2. Pipeline de clientes (CRM simples)
   - Quadro Kanban com as colunas: Prospeção, Contactado, Proposta enviada, Em negociação, Ganho, Perdido.
   - Cartão do cliente: nome, empresa, país, canal de contacto (LinkedIn, Upwork, Fiverr, email, outro), notas, data do próximo seguimento.
   - Arrastar cartões entre colunas.
   - Aviso visual nos cartões com seguimento em atraso.

3. Projetos
   - Criar projeto ligado a um cliente: título, serviço, valor, moeda, prazo, estado (Em curso, Entregue, Pago, Cancelado).
   - Lista com filtros por estado e cliente.

4. Faturas e pagamentos
   - Criar fatura a partir de um projeto: número automático, itens, moeda, data de emissão e vencimento.
   - Exportar a fatura em PDF, em português e opcionalmente em inglês.
   - Marcar como Paga e registar: valor recebido, método (PayPal, Wise, Payoneer, transferência SWIFT, outro), taxa cobrada e taxa de câmbio manual usada para AOA.
   - NÃO usar taxas de câmbio automáticas nem inventar valores: o utilizador introduz a taxa manualmente.

5. Painel de rendimento
   - Total recebido no mês e no ano, convertido para AOA com a taxa registada em cada pagamento.
   - Valor em atraso e valor por receber.
   - Gráfico de barras de rendimento por mês.
   - Rendimento por cliente e por tipo de serviço.
   - Total de taxas pagas, para o utilizador ver quanto "perde" por método de pagamento.

6. Modelos de mensagens
   - Biblioteca de modelos editáveis em português e inglês: primeira abordagem, proposta, seguimento, cobrança de fatura em atraso.
   - Botão "Copiar" em cada modelo e variáveis como {nome_cliente} substituídas automaticamente.

7. Perfil público (página simples)
   - O utilizador preenche nome, serviço, nicho, 3 a 5 trabalhos de portfólio (imagem, título, descrição) e contacto.
   - Gera uma página pública partilhável em /p/:username.

MODELO DE DADOS (Supabase)
Tabelas: profiles, clients, projects, invoices, invoice_items, payments, message_templates, portfolio_items.
Todas com user_id, created_at e RLS que restringe ao dono. Define relações e chaves estrangeiras de forma coerente.

DESIGN
- Estilo limpo e profissional, com bastante espaço em branco.
- Cor principal sóbria (azul-petróleo ou verde-escuro) e cor de destaque quente para ações principais.
- Tipografia legível, componentes shadcn/ui, ícones lucide-react.
- Navegação inferior no telemóvel e barra lateral no ecrã grande.
- Suporte a modo escuro.
- Estados vazios com mensagem útil e botão de ação (ex.: "Ainda não tens clientes. Adiciona o primeiro.").

STACK
React, TypeScript, Tailwind CSS, shadcn/ui, Supabase (auth e base de dados), recharts para gráficos.

REGRAS DE QUALIDADE
- Valida todos os formulários com mensagens de erro claras em português.
- Mostra estados de carregamento e erro em todas as listas.
- Nunca guardes dados sensíveis no código nem expons chaves.
- Código organizado em componentes pequenos e reutilizáveis.
- Não adiciones funcionalidades fora desta lista.

COMO TRABALHAR
Começa por: (1) autenticação, (2) layout e navegação, (3) pipeline de clientes. Depois pára e mostra o resultado antes de avançares para projetos, faturas e painel. Se algo estiver ambíguo, escolhe a opção mais simples e diz qual escolheste.

CRITÉRIOS DE ACEITAÇÃO
- Consigo registar-me, criar um cliente e movê-lo entre colunas do Kanban.
- Consigo criar um projeto e uma fatura em USD, marcá-la como paga com taxa e câmbio manual, e ver o valor em AOA no painel.
- Um segundo utilizador não consegue ver os dados do primeiro.
- Tudo funciona bem num ecrã de 375 px de largura.


# link
https://freela-ao-renda-clara.lovable.app
