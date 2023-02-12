
arquitetura MultiCloud 
composta por recursos rodando na AWS e Google Cloud Platform de forma 100% automatizada usando Terraform


./aws_set_credentials.sh accessKeys.csv    -> command  vai injetar o arquivo de chaves dos dados de informacao do  TF se autenticar com a AWS, esse script
vai injetar as informcaoes dentro do arquivo de conf do terraform para ele se autenticar com a conta dele na AWS



gcloud config set project <your-project-id>    -> vai set up o id dentro da google cloud para que o TF crie os recursos dentro deste projeto
  gcloud config set project durable-melody-377612
  
![image](https://user-images.githubusercontent.com/57456345/218312556-ef373987-a7c9-490a-8565-d0a4393ff822.png)

  
set up os id deste projeto dentro dos arquivos do TF
  ./gcp_set_project.sh
  
  com o output podemos ver que atualizou os arquivos do TF, assim pode autenticar e usar os recursos dentro da google cloud
  
  
  enable services, which we are goona use this project
  
  gcloud services enable containerregistry.googleapis.com
gcloud services enable container.googleapis.com
gcloud services enable sqladmin.googleapis.com
  
  
  
  
  
   bucket = "luxxy-covid-testing-system-pdf-pt-fabr757575"
  
  
  
  
  Execute os seguintes comandos para provisionar os recursos de infraestrutura
  init baixar os plugins para se comunicar com os provedores 
  
  
  
  habilitar um IP privado no SQLcloud para se comunicar com a aplicacao atraves de um IP privado
  habilitar no firewall um acesso remoto para acesso ao banco de dados
  este para eh apenas para fns educativos
  
  took me 15min
  ![image](https://user-images.githubusercontent.com/57456345/218315860-1d6154e4-e836-41bf-9693-8ce72716c576.png)

  
  
  Missão 2: Você vai, de forma prática, descobrir como fazer o processo de conversão de uma aplicação e 
  o seu Banco de Dados para rodar em cima desta arquitetura MultiCloud (AWS e Google Cloud) e terá contato 
  com tecnologias como Docker e Kubernetes neste percurso.
  
  
  
  aplicacao precisa se comunicar com o S3 aws de forma q nao seja publica, com isso precisa de uma forma a app se autentiucar para acessar o S3
  com isso temos q criar um usuario novo
  
  
  ![image](https://user-images.githubusercontent.com/57456345/218323763-c4da0f41-5c5c-45c4-86a2-d30ea87d76c1.png)

  
  
  
  
  mission 3
  
  Missão 3: Como último passo, você vai descobrir, de forma prática, como fazer o processo de migração dos arquivos da aplicação e os dados de seu 
  Banco de Dados de forma profissional para a 
  AWS e Google Cloud (arquitetura MultiCloud), que são habilidades essenciais no processo de migração para a Cloud requerido por grandes empresas.
  
  
  
  ![image](https://user-images.githubusercontent.com/57456345/218327050-e3af5718-2ee4-4ff4-a2c3-0cd401d19bde.png)
  
  
  
  pdf
  https://luxxy-covid-testing-system-pdf-pt-fabr757575.s3.amazonaws.com/s029s02-as0202o9s-s29312-as-annsh.pdf?AWSAccessKeyId=AKIAXUN2ZQUEEAZHEU4V&Signature=oVf1Xn8BYCiIPhAnAFvMfnLAARU%3D&Expires=1676227455
  
nome do bucket - nome do arquivo (s029s02-as0202o9s-s29312-as-annsh.pdf)  - url assinada (Signature), assinado pq eh um bucket assinado e precisa 
  
  
  
  
  
