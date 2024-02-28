# Ros_workshop

## Criar o pacote ros:
Primeiramente indicamos nossa workspace:
```
$ cd ~/workshop_ws/
$ source devel/setup.bash
```

Criando o pacote dentro da src:
```
$ cd ~/workshop_ws/src
```

Utilizar o catkin_create_pkg para criar o pacote:
```
$ catkin_create_pkg minicurso_package std_msgs rospy roscpp
```
Aonde o "minicurso_package" é o nome do nosso pacote e "std_msgs rospy roscpp" são dependencias usadas neste pacote.

Compilar o pacote usando o catkin_make: 
```
$ cd ~/workshop_ws/
$ catkin_make
```
Localizamos na src nossos pacote:
```
$ cd ~/workshop_ws/src
$ ls 
```
Aonde utlizamos o comando ls para ver dentro da pasta.

## Criando scripts
Há diversas maneiras de trabalhar, podemos trabalhar diretamente pelo terminal utilizando ferramentas de editor de texto como o gedit ou nano, como também podemos utilizar softwares como sublimeText ou Vscode. Para ficar mais claro irei utilizar o Vs Code.
Podemos fazer sua instalação utlizando: (Tutorial de instalação https://www.dio.me/articles/instalacao-vs-code-no-linux)
```
$ sudo snap install –classic code
```
Dentro do nosso pacote criamos a pasta scripts para manter a organização:
```
$ cd ~/workshop_ws/src/minicurso_package
$ mkdir scripts
$ cd scripts
```
Lembrando que sempre podemos apertar TAB para completar, caso não complete pode ser um *indicativo* de problema.

Podemos partir para o vscode utilizando o comando
```
$ code .
```
Para visualizar o pacote inteiro dentro do vscode podemos voltar e inicializar
```
$ cd ~/workshop_ws/src/minicurso_package
$ code .

```
## Scripts
Vamos criar um publisher para mandar uma variavel e um subscriber para receber esta variavel: (utilizando apenas o terminal)
```
$ cd ~/workshop_ws/src/minicurso_package/scripts
$ gedit publisher.py

```
Codigo do publisher.py:
```
#!/usr/bin/env python
import rospy  #biblioteca para o ros
from std_msgs.msg import Int32

#função para enviar a variavel
def enviar_variavel():
    rospy.init_node('var_node', anonymous=True) # iniciar o nodo, aonde 'var_node' é o nome do nosso nodo
    pub = rospy.Publisher('/variavel', Int32, queue_size=10) # Utiliza o rospy.Publisher para declarar nossa publicação, aonde '/variavel' é nosso topico Int32 é o tipo da mensagem 
    variavel = 0
    
    while not rospy.is_shutdown():
        pub.publish(variavel)
        rospy.loginfo("Valor da variavel: %d", variavel)
    
if __name__ == '__main__':
    try:
        enviar_variavel()
    except rospy.ROSInterruptException:
        pass

```
Utilizamos o chmod +x para dar permissão para o ros executar o codigo.
```
$ chmod +x publisher.py
```
Com o codigo do nosso publisher feito, vamos criar o subscriber para receber nossa variavel:
```
$ cd ~/workshop_ws/src/minicurso_package/scripts
$ gedit publisher.py

```
Codigo do subscriber.py:
```
#!/usr/bin/env python
import rospy
from std_msgs.msg import Int32

def callback(data):
    rospy.loginfo(rospy.get_caller_id() + "Estou recebendo a variavel %d", data.data) #printa no terminal 
    
#função para iniciar o nodo
def listener():
    rospy.init_node('listener_var_node', anonymous=True) #indica o nodo, aonde 'listener_var_node' é o nome do nosso nodo

    rospy.Subscriber("/variavel", Int32, callback) #se subscreve no nosso topico /variavel, declarada no codigo do publisher 
    rospy.spin() 

if __name__ == '__main__':
    listener()

```
Utilizamos da mesma maneira o chmod +x para dar permissão para o ros executar o codigo.
```
$ chmod +x subscriber.py
```




