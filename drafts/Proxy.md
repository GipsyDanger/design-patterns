Proxy
===
----------------------
### O problema da internet( internet problem)
##### Contexto


Imagine que você é um programador cuja tarefa foi prover uma API de acesso de serviços de internet(email, navegação, chats, etc, horários de acesso, gerenciamento de banda, etc). 
e a nível de implementação, temos a classe:

```
class InternetProvider {
    constructor(){
        console.log('internet liberada');
        this.webNavigationPolicies = [];
        this.emailsPolicies = [];
        this.torrentPolicies = [];
     }

    setWebNavigationPolicy(params){
        this.webNavigationPolicies = params;
    }

    setEmailsPolicy(params){
        this.emailsPolicies = params
    }

    setTorrentsPolicy(params){
        this.torrentPolicies = params;
    }

    getwebNavigationPolicies(){
        return this.webNavigationPolicies;
    }

    getEmailsPolicies(){
        return this.emailsPolicies;
    }

    getTorrentPolicies(){
        return this.torrentPolicies;
    }
}
```

Nesse momento é possível setar as políticas de acesso de cada usuário. O problema é que existe a certeza de 'sujar' o código com políticas de
acesso que não necessariamente são de responsabilidade da classe `InternetProvider`. Um exemplo do anti-padrão seria:



```
class InternetProvider {
    constructor(user){
        this.webNavigationPolicies = [];
        this.emailsPolicies = [];
        this.torrentPolicies = [];
        this.user = user; 
     }

    setWebNavigationPolicy(params){
        if(this.user.role == 'admin'){
            this.webNavigationPolicies = params;
        }else{
            throw new Error('este usuário não possui acesso à esta funcionalidade');
        }
    }

    // ...

    getwebNavigationPolicies(){
        if(this.user.role == 'admin'){
            return this.webNavigationPolicies;
        }else{
            throw new Error('este usuário não possui acesso à esta funcionalidade');
        }      
    }
}
```

O problema gerado com esta implementação é que à medida que as políticas de acesso mudam, fica mais difícil para o programador
acompanhar e manter o código criado e é aí que o Proxy entra.


#### Vantagens do Proxy
 - No caso do nosso exemplo, provê um proxy protetor para a classe original, removendo a responsabilidade da classe original do gerenciamento das políticas de acesso.
 - Em classes de gerenciamento de I/O, protege o objeto original de alterações até que o mesmo esteja pronto para uma nova interação
 - Um proxy, em outro contexto, é usado para cacheamento de dados, poupando o objeto original de alguma operação custosa (como por exemplo, acesso a dados 'pesados' ou 'caros', como bancos de dados e arquivos grandes).
 - Preserva o princípio 'Open-closed' das classes (as torna abertas para extensão e fechadas para modificação)
 - Preserva o princípio da Responsabilidade Única (Single Responsibility principle) do SOLID.

#### Desvantagens do Proxy
 - Não sei proxy é só amor <3
 
#### Definição de Proxy

Vejamos a definição de *Proxyy*
>“Fornecer um substituto ou marcador da localização de outro objeto para controlar o acesso a esse objeto.” [1].

Tendo essa definição como guia, podemos fazer assim:

```
/* InternetProvider volta à sua implementação original */

class InternetProvider {
    constructor(){
        console.log('internet liberada');
        this.webNavigationPolicies = [];
        this.emailsPolicies = [];
        this.torrentPolicies = [];
     }

    setWebNavigationPolicy(params){
        this.webNavigationPolicies = params;
    }

    setEmailsPolicy(params){
        this.emailsPolicies = params
    }

    setTorrentsPolicy(params){
        this.torrentPolicies = params;
    }

    getwebNavigationPolicies(){
        return this.webNavigationPolicies;
    }

    getEmailsPolicies(){
        return this.emailsPolicies;
    }

    getTorrentPolicies(){
        return this.torrentPolicies;
    }
}

/* InternetProviderProxy estende IntenretProvider e gerencia as políticas de acesso da classe pai. */

class InternetProviderProxy  extends InternetProvider{
    constructor(price){
        this.webNavigationPolicies = [];
        this.emailsPolicies = [];
        this.torrentPolicies = [];
        this.user = user; 
    }

    isAdmin(){
        return this.user.role == 'admin'
    }

    setWebNavigationPolicy(params){
        if(isAdmin()){
            this.webNavigationPolicies = params;
        }else{
            throw new Error('este usuário não possui acesso à esta funcionalidade');
        }
    }

    // ...

    getwebNavigationPolicies(){
        if(isAdmin()){
            return this.webNavigationPolicies;
        }else{
            throw new Error('este usuário não possui acesso à esta funcionalidade');
        }      
    }
```

### Outros exemplos
 - Javascript possui a classe Proxy, cuja implementação é um pouco diferente deste exemplo, mas o propósito é o mesmo: Proteger o objeto pai de impleemntações
 que não são de sua responsabilidade.





