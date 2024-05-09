name: automacao_efs
run-name: EFS 
on:
  push:
  branches:
    -  "main"
    paths
      - images/*.*
      - github/workflows/main.yml
jobs:
  efs-senai:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Intalação do ssh
        run: sudo apt-get install -y openssh-client

      -  name: Verificação da chave pem
      run: echo "$KEY_PATH"
      env:
        KEY_PATH: ${{ secrets.CHAVE_PEM }}


    - name: Adicionar host EC2 ao known_host  
      run: 
        ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa -N ''
        ssh-keyscan -H 44.222.158.123 >> ~/.ssh/known_hosts


        - name: Envio das imagens SCP
          if: always()
          run:
            echo "$KEY_PATH" > ./chave.pem
            chmod 600 ./chave.pem
            scp -i ./chave.pem images/*.* ec2-user@44.222.158.123:/home/ec2-user/efs
          env:
            KEY_PATH: ${{ secrets.CHAVE_PEM }}
            
            
        
        
