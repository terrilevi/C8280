Departamento Académico de Ingeniería C8280 -Comunicación de Datos y
Redes

"Año de la unidad, la paz y el desarrollo" UNIVERSIDAD PERUANA CAYETANO
HEREDIA

Facultad de Ciencias y Filosofía "Alberto Cazorla Talleri"

INGENIERÍA INFORMÁTICA Practica Calificada 4

Trabajo presentado por: Sophia Escalante Rodríguez

Profesor: Ing. Cesar Jesus Lara Avila

Fecha de entrega: 19 de junio del 2023

Amazon ELB Aquí, usamos Amazon Elastic Load Balancing (ELB) y Amazon
Cloud Watch a través de la CLI de AWS para equilibrar la carga de un
servidor web. Parte 1: ELB  1. Inicia sesión en el sandbox del curso AWS
. Ve al directorio donde guarda el archivo de script de instalación de
apache del laboratorio de la práctica calificada 3. Para crear un
balanceador de carga, haz lo siguiente. aws elb create-load-balancer
\--load-balancer-name sophia \--listeners
\"Protocol=HTTP,LoadBalancerPort=80,InstanceProtocol=HTTP,InstancePort=80\"
\--availability-zones us-east-1a ¿Cuál es el DNS_Name del balanceador de
carga?

\"DNSName\": \"sophia-1676645509.us-east-1.elb.amazonaws.com\" El campo
\"DNSName\" en la salida muestra el nombre DNS asignado al balanceador
de carga. En este caso, el nombre DNS del balanceador de carga sería
\"sophia1-2130411817.us-east-1.elb.amazonaws.com\". El nombre DNS del
balanceador de carga es importante porque es la dirección que se utiliza
para acceder a la aplicación o servicio que está detrás del balanceador
de carga. Al configurar el DNS para apuntar al nombre del balanceador de
carga, cualquier solicitud enviada a ese nombre será automáticamente
distribuida y manejada por el balanceador de carga, que luego las
enviará a las instancias subyacentes de manera equitativa.

2\. El comando describe-load-balancers describe el estado y las
propiedades de tu(s) balanceador(es) de carga. Presenta este comando.
aws elb describe-load-balancers \--load-balancer-name sophia1 ¿Cuál es
la salida?

Estoy utilizando el comando \"aws elb describe-load-balancers
\--load-balancer-name sophia1\" para obtener información sobre el
balanceador de carga llamado \"sophia1\" que creamos anteriormente.
Quiero ver cómo está configurado y obtener detalles importantes sobre
él.

3\. Creamos dos instancias EC2, cada una ejecutando un servidor web
Apache. Emite lo siguiente. aws ec2 run-instances \--image-id
ami-d9a98cb0 \--count 2 \--instance-type t1.micro \--key-name
sophia1-key \--security-groups sophia1 \--user-data
file://./apache-install \--placement AvailabilityZone=us-east-1a

¿Qué parte de este comando indica que deseas dos instancias EC2? La
parte del comando que indica que deseamos dos instancias EC2 es
\"\--count 2\". Este parámetro especifica el número de instancias que se
crearán.

¿Qué parte de este comando garantiza que tus instancias tendrán Apache
instalado? La parte del comando que garantiza que las instancias tendrán
Apache instalado es \"\--user-data file://./apache-install\". Este
parámetro proporciona un script de usuario que se ejecutará en las
instancias al iniciar. El script \"apache-install\" contiene las
instrucciones para instalar Apache.

Para estas dos preguntas usé el siguiente comando: aws ec2
describe-instances ¿Cuál es el ID de instancia de la primera instancia?
\"InstanceId\": \"i-0c5a0ab60776bff72\" \"PublicIpAddress\":
\"54.165.89.83\"

¿Cuál es el ID de instancia de la segunda instancia? \"InstanceId\":
\"i-047a269dcbb7c23d8\" \"PublicIpAddress\": \"3.92.56.91\"

4\. Para usar ELB, tenemos que registrar las instancias EC2. Haz lo
siguiente, donde instance1_id e instance2_id son los obtenidos del
comando en el paso 3. aws elb register-instances-with-load-balancer
\--load-balancer-name sophia \--instances i-0c5a0ab60776bff72
i-047a269dcbb7c23d8 ¿Cuál es la salida?

Este resultado significa que se han registrado correctamente dos
instancias en el balanceador de carga llamado \"sophia1\". Las
instancias están identificadas por los IDs \"i-0c5a0ab60776bff72\" e
\"i-047a269dcbb7c23d8\". Ahora vea el estado de la instancia de los
servidores cuya carga se equilibra. aws elb describe-instance-health
\--load-balancer-name sophia ¿Cuál es la salida?

Esta salida indica que las dos instancias con los IDs
\"i-047a269dcbb7c23d8\" e \"i-0c5a0ab60776bff72\" están en servicio
(\"InService\") en el balanceador de carga \"sophia\". Esto es algo
positivo, ya que significa que ambas instancias están disponibles y
listas para recibir tráfico del balanceador de carga.

5\. Abre el navegador del sandox. Recupera la dirección IP de tu
balanceador de carga del paso 1, ingresa la URL
http://sophia-1676645509.us-east-1.elb.amazonaws.com/ en tu navegador
web. ¿Qué apareció en el navegador?

Funcionó correctamente. Cuando accedo al URL del balanceador de carga en
tu navegador, tu solicitud se dirige primero al balanceador de carga. El
balanceador de carga evalúa la carga de cada una de las instancias EC2
registradas y decide a qué instancia enviará la solicitud. Esto permite
distribuir equitativamente la carga de trabajo entre las instancias y
optimizar el rendimiento.

6\. Abre dos ventanas de terminal adicionales y ssh en ambos servidores
web. En cada uno, cd al directorio DocumentRoot (probablemente
/usr/local/apache/htdocs) y modifique la página de inicio
predeterminada, index.html, de la siguiente manera.
\<html\>\<body\>\<h1\>¡Funciona!\</h1\> \<p\>La solicitud se envió a la
instancia 1.\</p\> \<p\>La solicitud fue atendida por el servidor web
1.\</p\> \</body\>\</html\> Para el segundo servidor, haz lo mismo
excepto que use la instancia 2 y el servidor 2 para las líneas 2 y 3.
Para el primer servidor:

Para el segundo servidor:

En el navegador web, accede a tu balanceador de carga 4 veces
(actualícelo/recárgalo 4 veces). Esto genera 4 solicitudes a tu
balanceador de carga. ¿Cuántas solicitudes atendió el servidor web 1?
Atendió dos solicitudes correctamente

¿Cuántas solicitudes atendió el servidor web 2? Atendió dos solicitudes
correctamente

Parte 2: CloudWatch 7. CloudWatch se utiliza para monitorear instancias.
En este caso, queremos monitorear los dos servidores web. Inicia
CloudWatch de la siguiente manera. aws ec2 monitor-instances
\--instance-ids i-0c5a0ab60776bff72 i-047a269dcbb7c23d8 ¿Cuál es la
salida? En la salida, se muestra el estado de monitoreo de dos
instancias. Cada instancia se identifica por su ID de instancia
(InstanceId) y se indica si el monitoreo está habilitado (State:
enabled) o no. Cuando el monitoreo está habilitado para una instancia de
EC2, significa que se está recopilando información detallada sobre su
rendimiento y estado. Esto incluye métricas como el uso de la CPU, la
utilización de la memoria, el tráfico de red y el rendimiento de los
discos.

Ahora examina las métricas disponibles con lo siguiente: aws cloudwatch
list-metrics \--namespaces \"AWS/EC2\" ¿Viste la métrica CPU Utilization
en el resultado?

Estos fragmentos de código describen una métrica llamada
\"CPUUtilization\" en el espacio de nombres \"AWS/EC2\" y proporciona
información sobre la instancia de EC2 específica a la que se refiere la
métrica. Esto es útil para identificar y monitorear el uso de la CPU de
una instancia específica en el entorno de EC2.

8\. Ahora configuramos una métrica para recopilar la utilización de la
CPU. Obtener la hora actual con date -u. Esta será tu hora de inicio. Tu
hora de finalización debe ser 30 minutos más tarde. Haz lo siguiente.
Ejecuté los siguientes comandos: Para la instancia
\"i-0c5a0ab60776bff72\": aws cloudwatch get-metric-statistics \\
\--metric-name CPUUtilization \\ \--start-time \"2023-06-19T18:21:00Z\"
\\ \--end-time \"2023-06-19T18:23:32Z\" \\ \--period 3600 \\
\--namespace AWS/EC2 \\ \--statistics Maximum \\ \--dimensions
Name=InstanceId,Value=i-0c5a0ab60776bff72 Para la instancia
\"i-047a269dcbb7c23d8\": aws cloudwatch get-metric-statistics \\
\--metric-name CPUUtilization \\ \--start-time \"2023-06-19T18:21:00Z\"
\\ \--end-time \"2023-06-19T18:23:32Z\" \\ \--period 3600 \\
\--namespace AWS/EC2 \\ \--statistics Maximum \\ \--dimensions
Name=InstanceId,Value=i-047a269dcbb7c23d8 ¿Cuál es la salida? Estas
salidas muestran la utilización máxima de la CPU para cada una de las
instancias \"i-0c5a0ab60776bff72\" e \"i-047a269dcbb7c23d8\" durante el
período de tiempo especificado. Los valores indican el porcentaje máximo
de utilización de la CPU registrado en ese lapso.

9\. Apache tiene un herramienta benchmark llamada ab. Si desea ver más
información sobre ab, consulte
http://httpd.apache.org/docs/2.0/programs/ab.html. Para ejecutar ab,
emita el siguiente comando en tu sistema de trabajo. ab -n 50 -c 5
http://sophia-1676645509.us-east-1.elb.amazonaws.com/ En este ejemplo,
se enviarán un total de 50 solicitudes con una concurrencia de 5. Lo que
hice fue utilizar una herramienta llamada ApacheBench (ab) para hacer
una prueba de rendimiento a un servidor web. Quiero evaluar cómo
responde el servidor cuando se le envían múltiples solicitudes al mismo
tiempo. Mi objetivo era ver qué tan rápido podía responder a las
solicitudes y cuántas solicitudes podía manejar en un segundo. Después
de realizar la prueba, obtuve algunos resultados interesantes. El tiempo
promedio que tardó cada solicitud en ser procesada fue de alrededor de
126.404 milisegundos (ms), lo cual me da una idea de la velocidad de
respuesta del servidor. Además, descubrí que el servidor pudo manejar
alrededor de 39.56 solicitudes por segundo en promedio. Esto significa
que tiene una capacidad decente para manejar un flujo constante de
solicitudes. Durante la prueba, se transfirieron un total de 593,674
bytes de datos desde el servidor al cliente. Esto incluye el contenido
HTML y otros recursos del sitio web. Sin embargo, también hubo algunas
solicitudes que fallaron, un total de 24 para ser exactos. Esto puede
deberse a problemas de conexión o errores en el servidor.

10\. Ahora queremos examinar la métrica de latencia del ELB. Utiliza el
siguiente comando con las mismas horas de inicio y finalización que
especificó en el paso 8. Métrica de latencia máxima del ELB:

aws cloudwatch get-metric-statistics \\ \> \--metric-name Latency \\ \>
\--start-time \"\$(date -u -d \'1 hour ago\' +\'%Y-%m-%dT%H:%M:%SZ\')\"
\\ \> \--end-time \"\$(date -u +\'%Y-%m-%dT%H:%M:%SZ\')\" \\ \>
\--period 300 \\ \> \--namespace AWS/ELB \\ \> \--statistics Average \\
\> \--dimensions Name=LoadBalancerName,Value=sophia

El resultado que obtuve muestra una lista de puntos de datos. Cada punto
de datos representa un momento en el tiempo en el que se registró la
métrica. Para cada punto de datos, se muestra la fecha y hora en la que
se registró, el promedio de latencia en ese momento y la unidad de
medida (segundos). En el resultado que mostré, se pueden ver varios
puntos de datos registrados en diferentes momentos durante la última
hora. Cada punto de datos nos da una idea del promedio de latencia en
ese momento.

Número total de solicitudes procesadas por el ELB: aws cloudwatch
get-metric-statistics \\ \> \--metric-name RequestCount \\ \>
\--start-time \"\$(date -u -d \'1 hour ago\' +\'%Y-%m-%dT%H:%M:%SZ\')\"
\\ \> \--end-time \"\$(date -u +\'%Y-%m-%dT%H:%M:%SZ\')\" \\ \>
\--period 300 \\ \> \--namespace AWS/ELB \\ \> \--statistics Sum \\ \>
\--dimensions Name=LoadBalancerName,Value=sophia

La respuesta del comando nos muestra una lista de puntos de datos. Cada
punto de datos representa un momento específico en el tiempo en el que
se registró el recuento de solicitudes. En cada punto de datos, se nos
proporciona la fecha y hora exacta en que se registró esa información.
Además, se nos muestra el recuento total de solicitudes en cada uno de
esos momentos. Por ejemplo, en el primer punto de datos, se registró un
recuento de 1 solicitud. En el segundo punto, se registraron 2
solicitudes, y así sucesivamente.

Parte 3: Limpieza 11. Necesitamos cancelar el registro de las instancias
de ELB. Haz lo siguiente: aws elb
deregister-instances-from-load-balancer \\ \--load-balancer-name sophia
\\ \--instances i-0c5a0ab60776bff72 i-047a269dcbb7c23d8 ¿Cuál es la
salida?

Se borraron satisfactoriamente. 12. A continuación, eliminamos la
instancia de ELB de la siguiente aws elb delete-load-balancer
\--load-balancer-name sophia

Finalmente, finaliza las instancias del servidor web de tus instancias y
tu instancia EC2. ¿Qué comandos usaste? ¿Cuál es la salida? aws ec2
terminate-instances \--instance-ids i-0c5a0ab60776bff72
i-047a269dcbb7c23d8 Este resultado indica que las instancias con los IDs
\"i-0c5a0ab60776bff72\" e \"i-047a269dcbb7c23d8\" han comenzado el
proceso de apagado y están en estado de \"shutting-down\", después de
haber estado previamente en estado de \"running\".

Auto Scaling Usamos AWS CLI para configurar sus instancias EC2 para el
escalado automático. Parte 1: escalar hacia arriba  1. Inicia sesión en
el sandbox virtual. Cambia al directorio donde guarda el archivo de
script de instalación de apache. Inicie una instancia de la siguiente
manera. aws autoscaling create-launch-configuration \\
\--launch-configuration-name sophia2-lc \\ \--image-id ami-d9a98cb0 \\
\--instance-type t1.micro \\ \--key-name sophia1-key \\
\--security-groups sophia1 \\ \--user-data file://./apache-install ¿Cuál
es la salida?

No hubo una salida directa, pero comprobé si se creó bien al utilizar el
siguiente comando: aws autoscaling describe-launch-configurations
\--launch-configuration-names sophia2-lc

A continuación, crea un equilibrador de carga. aws elb
create-load-balancer \--load-balancer-name sophia2 \--listeners
\"Protocol=HTTP,LoadBalancerPort=80,InstanceProtocol=HTTP,InstancePort=80\"
\--availability-zones us-east-1c ¿Cuál es la salida? El campo
\"DNSName\" en la salida muestra el nombre DNS asignado al balanceador
de carga. En este caso, el nombre DNS del balanceador de carga sería
\"sophia2-1572398577.us-east-1.elb.amazonaws.com\".

2\. Ahora creamos un grupo de escalado automático. Haz lo siguiente. aws
autoscaling create-auto-scaling-group \--auto-scaling-group-name
sophia2-asg \--launch-configuration-name sophia2-lc \--min-size 1
\--max-size 3 \--load-balancer-names sophia2 \--availability-zones
us-east-1c ¿Cuál es la salida? No hubo una salida directa, pero comprobé
si se creó bien al utilizar el siguiente comando: aws autoscaling
describe-auto-scaling-groups \--auto-scaling-group-names sophia2-asg

¿Cuál es el ID de la instancia? \"InstanceId\": \"i-0416645026e3b715e\"
4. Ahora creamos una política de escalado hacía arriba Al utilizar el
comando aws autoscaling put-scaling-policy, estoy creando una política
de escalado hacia arriba en AWS para mi grupo de escalado automático.
Esta política permite que AWS agregue automáticamente más instancias al
grupo cuando sea necesario para aumentar nuestros recursos. Cuando
ejecuto el comando, debo proporcionar algunos valores. El
\--auto-scaling-group-name debe ser reemplazado con el nombre de mi
grupo de escalado automático. El \--policy-name es el nombre que le doy
a la política para identificarla. Con \--scaling-adjustment establezco
cuántas instancias se agregarán cuando se active la política, y en este
caso, quiero agregar una instancia adicional. El \--adjustment-type
indica cómo se ajustará el número de instancias, y aquí estoy usando
ChangeInCapacity para ajustarlo en función de la diferencia de
capacidad. Además, utilizo \--cooldown para especificar un período de
enfriamiento de 120 segundos antes de que se puedan aplicar más acciones
de escalado. aws autoscaling put-scaling-policy
\--auto-scaling-group-name sophia2-asg \--policy-name sophia2-scaleup
\--scaling-adjustment 1 \--adjustment-type ChangeInCapacity \--cooldown
120 ¿Cuál es la salida de este comando?

Obtendré una respuesta que incluirá el PolicyARN, que es un
identificador único para esta política de escalado hacia arriba que he
creado ¿Cuál es el valor de PolicyARN? (El valor es una cadena larga sin
comillas)
arn:aws:autoscaling:us-east-1:815818028142:scalingPolicy:d852f535-66f6-474f-807d-e704c8a0629c:autoScalingGroupName/sophia2-asg:policyName/sophia2-scaleup

5\. Luego creamos una alarma de CloudWatch para determinar, en caso de
que la política sea cierta, que AWS necesita ampliar nuestros recursos.
aws cloudwatch put-metric-alarm \--alarm-name sophia2-highcpualarm
\--metric-name CPUUtilization \--namespace AWS/EC2 \--statistic Average
\--period 120 \--threshold 70 \--comparison-operator
GreaterThanThreshold \--dimensions
\"Name=AutoScalingGroupName,Value=sophia2-asg\" \--evaluation-periods 1
\--alarm-actions
arn:aws:autoscaling:us-east-1:815818028142:scalingPolicy:d852f535-66f6-474f-807d-e704c8a0629c:autoScalingGroupName/sophia2-asg:policyName/sophia2-scaleup
¿Cuál es la salida? Este comando, aws cloudwatch put-metric-alarm, nos
permite crear una alarma en CloudWatch en AWS. En este caso, la alarma
se establece para monitorear la utilización de la CPU en un grupo de
escalado automático. Si la utilización de la CPU supera el umbral del
70% durante un período de 120 segundos, la alarma se activará. En otros
términos, este comando establece una alarma en CloudWatch para
supervisar la utilización de la CPU en un grupo de escalado automático.
Si se supera el umbral establecido, se tomarán las acciones definidas,
como escalar automáticamente para agregar más instancias, con el
objetivo de asegurar un rendimiento adecuado y evitar problemas de
capacidad.

5\. Inicia sesión en la instancia EC2 desde el paso 1 mediante ssh.
Cambia al root. Cargaremos y ejecutaremos una herramienta stress para
aumentar la utilización de procesamiento del servidor. Emite los
siguientes comandos de Linux. ssh -i devenv-key.pem ubuntu@44.203.237.60
apt-get install stress

stress\--cpu 1 Al ejecutar el comando \"stress \--cpu 1\" desde la
conexión SSH a la instancia, lo que estoy haciendo es simular una carga
de trabajo en el sistema. En este caso, estoy indicando que se utilice 1
núcleo de CPU para generar actividad intensiva y poner a prueba la
capacidad de procesamiento de la instancia. El comando \"stress\" es una
herramienta que se utiliza para estresar y probar el rendimiento del
sistema. Al ejecutarlo con el argumento \"\--cpu 1\", le estoy diciendo
que se genere carga solo en un núcleo de CPU. Esta acción es útil para
verificar el funcionamiento del sistema bajo condiciones de alta carga y
monitorear cómo responde la instancia y el autoscaling en términos de
escalado automático, basado en las políticas y alarmas configuradas
previamente Inicia un nuevo terminal. Repite el siguiente comando cada 2
minutos hasta que veas la segunda instancia EC2 y luego una tercera
instancia EC2. Segunda instancia EC2:

En este punto, emite las siguientes instrucciones en la ventana de tu
terminal inicial. aws autoscaling describe-auto-scaling-groups
\--auto-scaling-group-name sophia2-asg

¿Cuál es la salida? Esta salida indica que el grupo de escalado
automático \"sophia2-asg\" tiene actualmente dos instancias en ejecución
(con los IDs \"i-0416645026e3b715e\" e \"i-0420498fe58da747d\") y está
configurado con un tamaño mínimo de 1 y un tamaño máximo de 3. Además,
está asociado con un balanceador de carga llamado \"sophia2\".

Parte 2: Reducir la escala 6. Ahora exploraremos cómo AWS puede
controlar el escalado hacia abajo mediante la eliminación de algunas de
las máquinas virtuales creadas. Ejecuta los siguientes dos comandos,
nuevamente tomando nota del PolicyARN creado a partir del primer comando
para usar en el segundo.

aws autoscaling put-scaling-policy \--auto-scaling-group-name
sophia2-asg \--policy-name sophia2-scaledown \--scaling-adjustment -1
\--adjustment-type ChangeInCapacity \--cooldown 120 ¿Cual es la salida?
¿Cuál es el valor de PolicyARN?

Obtendré una respuesta que incluirá el PolicyARN, que es un
identificador único para esta política de escalado hacia abajo que he
creado
arn:aws:autoscaling:us-east-1:815818028142:scalingPolicy:72ee9b24-30a9-493c-b360-f91b00a71d79:autoScalingGroupName/sophia2-asg:policyName/sophia2-scaledown

7\. Luego creamos una alarma de CloudWatch para determinar, en caso de
que la política sea cierta, que AWS necesita ampliar nuestros recursos.

Este comando crea una alarma de CloudWatch que se activará si la
utilización de la CPU es menor que 30 en el grupo de escalado automático
\"sophia2-asg\". La acción que se llevará a cabo cuando se active la
alarma se define mediante el valor de PolicyARN proporcionado. aws
cloudwatch put-metric-alarm \--alarm-name sophia2-lowcpualarm
\--metric-name CPUUtilization \--namespace AWS/EC2 \--statistic Average
\--period 120 \--threshold 30 \--comparison-operator LessThanThreshold
\--dimensions \"Name=AutoScalingGroupName,Value=sophia2-asg\"
\--evaluation-periods 1 \--alarm-actions
arn:aws:autoscaling:us-east-1:815818028142:scalingPolicy:72ee9b24-30a9-493c-b360-f91b00a71d79:autoScalingGroupName/sophia2-asg:policyName/sophia2-scaledown
¿Cual es la salida? La salida indica que la alarma
\"sophia2-lowcpualarm\" está en estado de alarma debido a que la
utilización de la CPU fue inferior al umbral establecido y las acciones
configuradas, como la política de escalado hacia abajo, se activarán en
respuesta a esta condición.

8\. Cambia al terminal de la instancia EC2. Escribe ctrl+c para detener
el comando stress. Vuelva a la ventana del terminal y repite el
siguiente comando cada 2 minutos hasta que vea solo un EC2 en su grupo
de escalado automático. aws autoscaling describe-auto-scaling-groups
\--auto-scaling-group-name sophia2-asg ¿Cuál es la salida? La salida
indica que el grupo de escalado automático \"sophia2-asg\" está
configurado con una capacidad deseada de 1 y tiene una instancia en
ejecución en este momento.

Parte 3: Limpieza 8. Elimina el grupo de escalado automático mediante el
siguiente comando. Si el grupo tiene instancias o actividades de
escalado en curso, debes especificar la opción para forzar la
eliminación para que se realice correctamente. Si el grupo tiene
políticas, al eliminar el grupo se eliminan las políticas, las acciones
de alarma subyacentes y cualquier alarma que ya no tenga una acción
asociada. aws autoscaling delete-auto-scaling-group
\--auto-scaling-group-name sophia2-asg \--force-delete

Notamos que se eliminó el grupo de escalado llamado sophia2-asg en la
segunda imagen. 9. Después de eliminar tu grupo de escala, elimina tus
alarmas como se muestra a continuación.

aws cloudwatch delete-alarms \--alarm-name sophia2-lowcpualarm aws
cloudwatch delete-alarms \--alarm-name sophia2-highcpualarm

Notamos que se eliminaron las dos alarmas previamente creadas en la
imagen mostrada. 10. Elimina tu configuración de lanzamiento de la
siguiente manera. aws autoscaling delete-launch-configuration
\--launch-configuration-name sophia2-lc ¿Cuál es la salida?

Verificar si la configuración de lanzamiento \"sophia2-lc\" ya no está
presente en la lista de configuraciones de lanzamiento. Si no se
encuentra, eso significa que se eliminó correctamente.

Finalmente, elimina tu ELB. ¿Qué comando ejecutaste?. aws elbv2
delete-load-balancer \--load-balancer-arn \<ARN_del_ELBA\>
