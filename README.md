Nome do TCC : SISTEMA WEB INTEGRADO PARA GESTAO DE ESTÁGIOS DE ESCOLAS TÉCNICAS.
Integrantes to TCC de 2026 : Bianca Uchôas Fernandes, Maria Rita Lima Barbosa Oliveira e Karine Flores dos Santos.

Minha função no trio do TCC : programação backend e frontend do projeto.


Sistema web para gestão de estágios obrigatórios, desenvolvido como Trabalho de Conclusão de Curso (TCC) do curso técnico em Informática da Univap. O sistema centraliza todo o fluxo de estágio: da publicação de vagas pelas empresas/admins até a geração automática do Relatório Final de Estágio em PDF, seguindo as normas da instituição.

## Sobre o projeto* 

O estágio obrigatório envolve três perfis de usuário, cada um com uma tela e um conjunto de permissões próprios:

- **Aluno (estagiário):** se candidata a vagas, registra diariamente as atividades e horas do estágio, acompanha a carga horária cumprida e gera o Relatório Final em PDF ao concluir o estágio.
- **Admin:** publica e gerencia vagas de estágio, só podendo editar/encerrar as vagas que ele mesmo criou.
- **Gestão (coordenação acadêmica):** tem visão geral do sistema — gerencia alunos, cursos, admins, avisos e acompanha os relatórios de todos os alunos.

O projeto nasceu da necessidade de substituir o controle manual (planilhas, fichas impressas) por um fluxo digital único, reduzindo erros de preenchimento e garantindo que a documentação final do estágio saia pronta e dentro do padrão exigido pela instituição.

## 🚀 Funcionalidades

### Autenticação e perfis de usuário
- Login único com JWT, redirecionando para a tela correspondente ao tipo de usuário (aluno, admin ou gestão).
- Contas de admin e gestão nunca são excluídas do sistema — apenas **ativadas/desativadas**, preservando o histórico. O mesmo vale para o aluno, cujo estágio e horas registradas não podem ser perdidos.

### Vagas de estágio
- Cadastro de vaga com descrição, requisitos, benefícios, modelo (presencial/híbrido/remoto), turno, carga horária, bolsa, dados da empresa e cursos aceitos.
- Listagem paginada, sempre ordenada das vagas mais recentes para as mais antigas.
- Edição e encerramento de vaga restritos ao admin que a publicou (um admin não consegue mexer na vaga de outro).

### Registro diário de atividades (aluno)
- O aluno registra, dia a dia, a atividade realizada e as horas trabalhadas, com validações automáticas:
  - Horas em formato livre (ex: 5h35min), não apenas números redondos — a soma é feita corretamente considerando os minutos.
  - Máximo de 30 horas por semana.
  - Não é possível registrar atividade em sábados ou domingos.
  - Descrição limitada a 180 caracteres.
- Acompanhamento visual da carga horária cumprida vs. carga horária obrigatória do curso, com barra de progresso.
- Exclusão de um registro específico ou de todos os registros de uma vez.

### Relatório Final de Estágio (PDF)
Gerado automaticamente pelo sistema (jsPDF), reunindo:
- Capa, sumário com numeração dinâmica de páginas e identificação do aluno/empresa.
- Histórico admissional e atividades desenvolvidas.
- **Anexo A** — Termo de Compromisso de Estágio.
- **Anexo B** — Apólice de Seguro.
- **Anexo C** — Parecer Técnico do Supervisor (única página sem o cabeçalho institucional, pois deve ser impressa no papel timbrado da empresa concedente).
- **Anexo D** — Ficha de Avaliação Final de Estágio, com assinatura do aluno e do supervisor.
- **Anexo E** — Controle de Horas, dividido quinzenalmente (1ª e 2ª quinzena de cada mês), com campo de assinatura em todas as páginas.
- **Anexo F** — Portfólio (exclusivo para o curso de Publicidade).
- Instruções de entrega (encadernação, prazo, documentos complementares) exibidas ao final da geração.

### Gestão acadêmica
- Cadastro e edição de alunos, incluindo importação em massa via planilha.
- Cadastro de cursos com nome, cor de identificação e carga horária obrigatória — qualquer alteração se reflete automaticamente para todos os alunos daquele curso.
- Publicação de avisos/comunicados para os alunos.
- Configuração das informações institucionais (usadas no PDF quando o estágio é realizado na própria instituição de ensino).
- Visão consolidada dos relatórios de horas de todos os alunos, sempre ordenados do mais recente para o mais antigo.

## 🛠️ Tecnologias utilizadas

**Backend**
- [Node.js](https://nodejs.org/) + [Express](https://expressjs.com/)
- [MySQL](https://www.mysql.com/) com o driver [mysql2](https://www.npmjs.com/package/mysql2)
- [JWT (jsonwebtoken)](https://www.npmjs.com/package/jsonwebtoken) para autenticação
- [bcrypt](https://www.npmjs.com/package/bcrypt) para hash de senhas
- Arquitetura em camadas: **Rotas → Controllers → Services → Models**, separando validações de negócio do acesso ao banco

**Frontend**
- HTML, CSS e JavaScript puro (sem frameworks), com chamadas assíncronas (`fetch`) ao backend
- [jsPDF](https://github.com/parallax/jsPDF) + [jsPDF-AutoTable](https://github.com/simonbengtsson/jsPDF-AutoTable) para a geração do Relatório Final em PDF direto no navegador

**Banco de dados**
- MySQL relacional, com integridade referencial entre alunos, cursos, vagas e registros de horas (ex: a carga horária obrigatória é definida uma única vez no curso e herdada por todos os alunos via `JOIN`, nunca duplicada por aluno)

## 📁 Estrutura do projeto

```
sistema-estagios/
├── backend/
│   ├── config/          # Configuração da conexão com o banco de dados
│   ├── controllers/      # Recebem a requisição HTTP e devolvem a resposta
│   ├── services/         # Regras de negócio e validações
│   ├── models/           # Consultas SQL (mysql2)
│   ├── routes/           # Definição dos endpoints da API
│   ├── middlewares/       # Autenticação (JWT), tratamento de erros, upload
│   └── server.js         # Ponto de entrada do backend
└── frontend/
    ├── index.html         # Tela principal (pós-login)
    ├── login.html         # Tela de login
    ├── script.js     # Lógica do frontend (renderização, chamadas à API, geração do PDF)
    └── css/               # Estilos
```


## 📄 Licença

Projeto acadêmico, desenvolvido para fins educacionais (TCC)
