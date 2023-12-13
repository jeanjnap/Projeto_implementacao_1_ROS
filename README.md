# Projeto de implementação 1

Esse Readme contém o passo a passo para clonar e rodar o projeto, a metodologia abordada e os resultados obtidos

# Metodologia
Visando resolver um problema comum na robótica que é a navegação autônoma, implementamos um algoritmo de planejamento de trajetória para definir um caminho a ser percorrido de um ponto inicial até um ponto final. Para isso optamos por escolher o algoritmo AStar.

## Funcionamento do algoritmo

Para criar uma trajetória entre o ponto de origem e o destino, precisamos de um mapa do terreno onde será feita a trajetória, um ponto inicial e um ponto final. O mapa deve conter as informações dos obstáculos que devem ser evitados para traçar uma rota válida.
![image](https://github.com/jeanjnap/Projeto_implementacao_1_ROS/assets/26336215/fbc891ef-9844-44be-aa91-66ef7a2ba6de)

O algoritmo irá pegar os pixels da imagem do mapeamento e converter em uma matriz binária, onde os píxels escuros são marcados como 1 e representam os caminhos bloqueados.

![image](https://github.com/jeanjnap/Projeto_implementacao_1_ROS/assets/26336215/7d11d2b5-321a-475d-926c-d99dfcead0dc)

Que pode ser representado programaticamente por esse JSON

![image](https://github.com/jeanjnap/Projeto_implementacao_1_ROS/assets/26336215/06104b89-f185-4bd1-b03d-f5a0e22eb5fe)

Dado o exemplo anterior definimos o ponto inicial em azul e o final em vermelho

![image](https://github.com/jeanjnap/Projeto_implementacao_1_ROS/assets/26336215/8dd43ee6-a3ac-4bf4-8766-bcc9ba4ecb46)

Definimos o peso para se deslocar de um pixel para outro como 1, e sabendo que cada passo podemos nos mover apenas horizontalmente ou verticalmente podemos calcular o valor total da distância da trajetória.

Nos primeiros passos podemos nos mover apenas para baixo, pois temos uma parede que nos impede de nos movermos para a direita e nos pixels para a esquerda ou para cima estão fora dos limites do mapa.

![image](https://github.com/jeanjnap/Projeto_implementacao_1_ROS/assets/26336215/8e205405-d58c-4173-a5c0-40f01f4eff3a)

Temos também as cores laranja para demostrar o pixel atual e a cor amarela para demonstrar o caminho já percorrido.

No passo atual podemos ir tanto para baixo como para a direita, para escolher qual pixel deve ser o próximo, o algoritmo usa o cálculo de distância euclidiana, usando como base a posição das linhas e colunas para medir qual o pixel mais próximo do final, levando em consideração os pixels vizinhos disponíveis.

![image](https://github.com/jeanjnap/Projeto_implementacao_1_ROS/assets/26336215/1a4a60d1-f9e2-4da3-a683-c1199459b7d0)![image](https://github.com/jeanjnap/Projeto_implementacao_1_ROS/assets/26336215/ef4b2dc9-0947-4b27-a2e5-a42b124808b9)


Fonte (https://www.docentes.univasf.edu.br/carlos.freitas/geometria_analitica/distancia.php)

Após os cálculos, podemos ver que o pixel marcado em roxo claro tem uma distância menor até o pixel final em comparação com o pixel marcado em azul claro.

![image](https://github.com/jeanjnap/Projeto_implementacao_1_ROS/assets/26336215/71b47433-ad33-4dd0-8149-5c15d79f5a84)

Assim continuamos com o algoritmo até chegar ao ponto final.

![image](https://github.com/jeanjnap/Projeto_implementacao_1_ROS/assets/26336215/ade901ce-a922-4175-8de9-4b13068f77bf)

![image](https://github.com/jeanjnap/Projeto_implementacao_1_ROS/assets/26336215/a2eec588-0cd0-4536-9466-0357d2943934)

![image](https://github.com/jeanjnap/Projeto_implementacao_1_ROS/assets/26336215/6970bd80-21f2-40b5-8d82-7a659779d319)

![image](https://github.com/jeanjnap/Projeto_implementacao_1_ROS/assets/26336215/12c846d5-dde6-440a-acda-1217e5c4f96e)

![image](https://github.com/jeanjnap/Projeto_implementacao_1_ROS/assets/26336215/b71614f3-12bf-46f2-bd24-49f9cc5ac104)

Após todo esse processamento temos enfim uma rota definida.


# Configuração e execução
## Clonando o projeto
entre no seu workspace/src e rode o seguinte comando
```bash
git clone https://github.com/jeanjnap/Projeto_implementacao_1_ROS.git
```

Baixe o ROS Navigation Stack
```bash
sudo apt-get install ros-noetic-navigation
```

## Configurando e executando o projeto
1. Abra o arquivo 
ros_world/worlds/dc_new.world

Substitua o caminho dos arquivos nas 6 ocorrencias de 'home/jean/workspace/src/turtlebot3/meshes' para a localização do arquivo correspondente na sua workspace

![trab_robotica](https://github.com/jeanjnap/Projeto_implementacao_1_ROS/assets/26336215/a75bf981-d8f2-4d98-90f6-6a703ab50541)

2. Compilando o projeto

```bash
catkin build
source devel/setup.bash
```

3. Iniciando a simulação do Gazebo e Rviz

```bash
roslaunch ros_world gazebo.launch
```

4. Iniciando o script de planejamento de trajetória
Após a abertura do Gazebo e Rviz abra um novo terminal no diretório 'global_path_planning/scripts' e rode o seguinte comando

```bash
python3 path_planning_server.py
```

5. Na janela do Rviz clique em '2D Nav Goal' e clique no ponto que deseja que o robô vá.
Um ponto será marcado em vermelho (destino) e um ponto azul (origem) e logo em seguida será iniciada a criação da trajetória até encontrar o um caminho até o ponto de destino, assim que terminar será criada a rota em verde e o robô seguirá a rota
