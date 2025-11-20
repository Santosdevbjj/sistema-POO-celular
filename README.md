## Criando um Sistema e Abstraindo um Celular com POO em C#

![Screenshot_20250613-154319](https://github.com/user-attachments/assets/d8b60a21-f60e-47aa-a3aa-ee9e5582762b)


## Bootcamp WEX - End to End Engineering


## Descrição:
Neste LAB você será responsável por modelar um sistema que trabalha com celulares.

Para isso, foi solicitado que você faça uma abstração de um celular e disponibilize maneiras de diferentes marcas e modelos terem seu próprio comportamento, possibilitando um maior reuso de código.



## Desafio de projeto:
Para este desafio, você precisará usar seus conhecimentos adquiridos no módulo de orientação a objetos, da trilha C#


## Contexto:
Você é responsável por modelar um sistema que funciona com celulares. 

Para isso, foi solicitado que você faça uma abstração de um celular e disponibilize formas de diferentes marcas e modelos tendo seu próprio comportamento, possibilitando um maior uso de código e usando a orientação a objetos.


## Proposta:
Você precisa criar um sistema em C#, do tipo console, mapeando uma classe abstrata e classes específicas para dois tipos de celulares: Nokia e iPhone. Você deve criar suas classes de acordo com o diagrama de classes abaixo:


**Classe Smartphone** <<abstract>
+ Numero: string 
- modelo: string 
- IMEI: string
- Memoria: int 

+ Ligar()
+ ReceberLigacao()
+ InstalarAplicativo(string nome)<<abstract>> 



**Classe Nokia**
+ InstalarAplicativo(string nome)


**Classe Iphone**
+ InstalarAplicativo (string nome)


![Screenshot_20250613-171307](https://github.com/user-attachments/assets/e4369031-feea-44ef-ad30-f7f941916275)





## Regras e validações:

A **classe Smartphone deve ser abstrata,** não permitindo instanciar e operar apenas como modelo.

A **classe Nokia** e **Iphone** devem ser **classes filhas de Smartphone**.

O **método de InstalarAplicativo** deve ser escrito na classe Nokia e iPhone, pois ambos possuem maneiras diferentes de instalar um aplicativo. 


---

**Contato:**

[![Portfólio Sérgio Santos](https://img.shields.io/badge/Portfólio-Sérgio_Santos-111827?style=for-the-badge&logo=githubpages&logoColor=00eaff)](https://santosdevbjj.github.io/portfolio/)
[![LinkedIn Sérgio Santos](https://img.shields.io/badge/LinkedIn-Sérgio_Santos-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/santossergioluiz) 


---



