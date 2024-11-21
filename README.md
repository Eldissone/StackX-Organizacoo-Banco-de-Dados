# StackX-Organizacoo-Banco-de-Dados

# 1.Tabela `Usuarios`
Armazena informações dos usuários, que podem ser tanto administrador quanto usuarios normais “hóspedes”. </b>

CREATE TABLE Usuarios</b>
        (
        	id_aluno int PRIMARY KEY NOT NULL,
        	nome varchar(100) NOT NULL,
                email  varchar(30) NOT NULL,
        	senha varchar(50) NOT NULL,
        	sexo varchar(1) NOT NULL,
        	data_cadastro datetime NOT NULL,
        	tipo_usuario enum NOT NULL
        );

A tabela `Usuarios` é fundamental para armazenar dados de identificação e tipo de usuário, já que os mesmos podem ser tanto admin quanto hóspedes. 
O tipo de usuário ajuda a diferenciar permissões e recursos oferecidos.</b></b>

# 2. Tabela `Lugares_para_se_hospedar`
Armazena os lugares disponíveis para hospedagem, com informações detalhadas sobre o local.

CREATE TABLE Lugares_para_se_hospeda
        (
                id_lugar int PRIMARY KEY NOT NULL,
                id_admin int FOREIGN KEY NOT NULL,
                endereco varchar(50) NOT NULL,
                cidade varchar(50) NOT NULL,
                estado varchar(50) NOT NULL,
                pais varchar(30) NOT NULL,
                descricao varchar(50) NOT NULL,
                preco_diaria DECIMAL NOT NULL,
                capacidade int NOT NULL,
                data_cadastro datetime NOT NULL
        );

Cada lugar cadastrado pertence a um admin, o que explica a coluna `id_admin` como chave estrangeira. 
É importante armazenar dados detalhados do local para suportar pesquisas e informações para os hóspedes.

# 3. Tabela `Hospedagens`
Registra as hospedagens realizadas pelos usuários, que incluem informações sobre datas e status.

CREATE TABLE Hospedagens
      (
      	id_hospedagem int PRIMARY KEY NOT NULL,
      	id_usuario int FOREIGN KEY NOT NULL,
      	id_lugar int FOREIGN KEY NOT NULL,
      	data_inicio date NOT NULL,
        data_fim date NOT NULL,
      	status enum NOT NULL, --Status da hospedagem (`pendente`, `confirmado`, `cancelado`)
      );

Cada hospedagem conecta um hóspede a um lugar para um período específico.
As colunas `id_usuario` e `id_lugar` estabelecem as relações necessárias, e o status da hospedagem é importante para o gerenciamento das reservas.

# 4. Tabela `Avaliacoes`
Armazena as avaliações feitas por usuários sobre as hospedagens.

CREATE TABLE Avaliacoes
    (
      	id_avaliacao int PRIMARY KEY NOT NULL,
      	id_hospedagem int FOREIGN KEY NOT NULL,
      	id_usuario int FOREIGN KEY NOT NULL,
      	nota int NOT NULL, --Nota dada pelo usuário (1 a 5)  
        comentario trext NOT NULL,
        data_avaliacao datetime NOT NULL
      );
        
A tabela `Avaliacoes` precisa conectar-se tanto à hospedagem quanto ao usuário que realizou a avaliação. 
Isso permite vincular as avaliações a reservas específicas e identificar quem fez a avaliação, o que é importante para a transparência do sistema e para evitar avaliações falsas.



# Relacionamento
1. `Usuarios` > `Lugares` (1:N): Um usuário pode cadastrar vários lugares, mas cada lugar pertence a um único usuário, que é o admin.
2.  A chave estrangeira `id_admin` na tabela `Lugares` faz esse relacionamento.

3. `Usuarios` > `Hospedagens` (1:N): Um usuário pode realizar várias hospedagens (reservas), mas cada hospedagem pertence a um único usuário (hóspede).
4. O campo `id_usuario` na tabela `Hospedagens` faz a relação.

5. `Lugares` > `Hospedagens` (1:N): Um lugar pode ser reservado em diferentes momentos por diferentes hóspedes, mas cada hospedagem é referente a um único lugar.
6. A chave estrangeira `id_lugar` em `Hospedagens` estabelece essa conexão.

7. `Hospedagens` > `Avaliacoes` (1:1): Uma avaliação está vinculada a uma hospedagem específica, garantindo que cada reserva possa ter uma avaliação única.
8.  O campo `id_hospedagem` em `Avaliacoes` reflete esse vínculo.

9. `Usuarios` > `Avaliacoes` (1:N): Cada usuário pode fazer várias avaliações, mas uma avaliação pertence a um único usuário.
10.  O campo `id_usuario` em `Avaliacoes` implementa esse relacionamento.


