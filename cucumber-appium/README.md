# cucumber-appium-ruby-exemplo

Esta é uma estrutura de exemplo para executar testes BDD automatizados para Android e iOS.

## Pré Requisitos

* Mac
* Android SDK
* Xcode installed
* Ruby 2.7.1
* Appium

### Instalar o Appium e Dependencias 
Isso instalará a versão de linha de comando do appium.

    brew install node
    npm install -g appium@1.4.0
    npm install wd

Clone a estrutura

// 


Navegue até o diretório da estrutura

    cd cucumber-appium-ruby-example`


Instale o bundler e as gems das quais a estrutura depende
    gem install bundler
    bundle install

## Executando os Testes
* 
O servidor Appium deve estar em execução, a partir do terminal ou GUI.

    appium

Use Cucumber para executar os testes.

    pacote exec pepino -r recursos -p {CUCUMBER_PROFILE_HERE}


Perfis do cucumber são listados em `cucumber.yml`.

### Android-specific features
Para utilizar os recursos específicos do Android (logcat, gravação de tela em caso de falha), você deve fornecer ao teste o número de série do dispositivo ou UDID. Isso deve ser feito com `udid = {DEVICE_SERIAL}`.

Exemplo:

    pacote exec pepino -r recursos -p android_ci udid = HT293479

### Repetições
Ao executar os testes no Jenkins, descobrimos que às vezes os testes falhavam, seja devido à instabilidade do Appium ou o dispositivo não respondia aos toques, etc.

Para atender a isso, podemos usar reprises. Cada nova execução executará apenas os testes com falha da execução anterior. Se não houver nenhum teste com falha na execução inicial, as novas execuções serão executadas, mas nenhum teste será executado e será aprovado.

Exemplo de uso em que duas (no máximo) execuções são executadas:

    bundle exec cucumber -r features -p android_ci
    bundle exec cucumber -r features -p android_ci_rerun @rerun.txt
    bundle exec cucumber -r features -p android_ci_rerun2 @rerun2.txt

### Iniciando o Appium Server de dentro da estrutura

* A versão de linha de comando do appium deve ser instalada.

Isso pode ser útil para executar os testes de um CI Server, se você não quiser ter que iniciar e parar o servidor appium como uma tarefa separada.

Ao iniciar os testes, dê o parâmetro `START_APPIUM = true` e você também pode passar quaisquer parâmetros para o servidor appium.

Exemplo:

    bundle exec cucumber -r features -p android_ci START_APPIUM = true APPIUM_PARAMETERS = "- depuração em nível de log"

### APK e pacote personalizados
Normalmente, o pacote do aplicativo é definido em `env.rb` e o caminho para o aplicativo é definido em` props.yml`. No entanto, você pode, se necessário, especificá-los como parâmetros ao iniciar os testes.

Exemplo:

    bundle exec cucumber -r features -p android_ci apk = http: // localhost: 8080 / jobs / app / lastsuccessfulbuild / build.apk package = com.app.package

* Para o pacote iOS não é necessário, apenas o caminho para o próprio ipa pode ser fornecido como parâmetro.