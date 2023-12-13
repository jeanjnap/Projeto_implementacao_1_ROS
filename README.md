# Projeto de implementação 1

Esse Readme contém o passo a passo para clonar e rodar o projeto, a metodologia abordada e os resultados obtidos

## Clonando o projeto
entre no seu workspace/src e rode o seguinte comando
```bash
git clone https://github.com/jeanjnap/Projeto_implementacao_1_ROS.git
```

Baixe o ROS Navigation Stack
```bash
sudo apt-get install ros-noetic-navigation
```

## Alterando o caminho das peças do robô

1. Abra o arquivo 
ros_world/worlds/cd_new.world

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
