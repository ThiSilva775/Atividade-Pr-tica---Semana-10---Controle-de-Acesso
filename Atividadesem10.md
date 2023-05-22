Curso de Engenharia de Software - UniEVANGÉLICA 
Disciplina de Sisetmas Gerenciadores de Banco de Dados 
Dev: Thiago Silva Soares 
DATA : 21/05/2023

Seguem abaixo alguns exemplos de como podemos aplicar esses recursos:

1. Criação da view de relatório:
```
CREATE VIEW relatorio_treinos AS
SELECT
  u.nome AS nome_usuario,
  t.nome AS nome_treino,
  e.nome AS nome_exercicio,
  eq.nome AS nome_equipamento,
  et.ordem_exercicio,
  et.series,
  et.repeticoes
FROM
  Treino t
  JOIN Usuario u ON t.usuario_id = u.id
  JOIN ExercicioTreino et ON t.id = et.treino_id
  JOIN Exercicio e ON et.exercicio_id = e.id
  LEFT JOIN EquipamentoTreino eqt ON t.id = eqt.treino_id
  LEFT JOIN Equipamento eq ON eqt.equipamento_id eq.id;
```

2. Criação de um usuário que só pode visualizar os dados na view criada:
```
CREATE LOGIN usuario_relatorio WITH PASSWORD = 'senha123';
CREATE USER usuario_relatorio FOR LOGIN usuario_relatorio;
GRANT SELECT ON relatorio_treinos TO usuario_relatorio;
```

3. Criação de um usuário com permissão apenas de leitura em todo banco de dados:
```
CREATE LOGIN usuario_leitura WITH PASSWORD = 'senha456';
CREATE USER usuario_leitura FOR LOGIN usuario_leitura;
GRANT SELECT TO usuario_leitura;
```

Dessa forma, criamos uma view de relatório que contém informações sobre treinos, exercícios e equipamentos. Em seguida,amos um usuário específico que só pode visualizar os dados nessa view, garantindo a privacidade das informações. Por fim, criamos um usuário com permissão apenas de leitura em todo o banco de dados, garantindo que ele não possa modificar ou excluir informações importantes.