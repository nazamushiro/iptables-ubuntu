FIREWALL PARA UBUNTU SERVER

Está funcionando 100%, mais estou melhorando cada vez mais, na medida do possível.

Principais dúvidas:

O iptables protege contra vulnerabilidades do kernel?

Resposta: Não, o iptables não protege você de vulnerabilidades do kernel. Na verdade, o iptables também pode se tornar um vetor de ataque.

Existem outros lugares onde vulnerabilidades exploráveis ​​podem ocorrer:

O driver de rede: http://www.cvedetails.com/cve/CVE-2009-1389/

O próprio daemon SSH: http://www.openssh.com/security.html

Se você tiver sshd configurado com senhas fracas e alguém conseguir fazer força bruta, então exploits de escalonamento de privilégios locais podem ser tentados. O firewall normalmente não consegue capturar esse tipo de ataque, pois ele explora uma vulnerabilidade do aplicativo.

A melhor maneira de se proteger contra isso é tomando medidas preventivas, como não usar senhas fracas, não expor o host à Internet, a menos que seja necessário, manter-se atualizado aplicando patches de software, mudar a porta do ssh, instalar o Fail2Ban, etc. Fazendo isso já vai dar uma boa segurança.

iptables para Ubuntu Server Inspirado em "Vou revelar os iptables mais fortes de todos os tempos".

Como usar:
----
Clone, Edite e Execute.

cd /root

git clone https://github.com/lobo360/iptables-ubuntu

vim iptables.sh

chmod +x iptables.sh

sudo ./root/iptables.sh

Pronto.


---


Bom reserve um tempo para ler esse teste abaixo é interessante e pode clarear sua visão sobre os problemas relacionados a segurança da informação.

Exploits e Vulnerabilidades

Outro dia eu estava lendo sobre como vulnerabilidades de um sistema operacional podem ser usadas para Exploits.
Há algumas coisas que eu não entendo exatamente:
Se todas as conexões de entrada (exceto relacionadas ou estabelecidas) são negadas e todas as conexões de encaminhamento são negadas, como isso pode ser usado como uma vulnerabilidade? Eu acho que ninguém poderia entrar no sistema operacional. Certo?

Um firewall de software como nftables ou iptables é o primeiro ponto de entrada ou este é o sistema operacional que passa os pacotes para o firewall primeiro?

Relacionado à pergunta anterior, como um sistema operacional pode ser comprometido se você tiver que passar pelo firewall primeiro? Você teria que encontrar uma vulnerabilidade no firewall então certo!, ou não?

Se um site malicioso fizer você baixar um arquivo, e você tiver um navegador como o Firefox que pergunta onde colocar o arquivo, você não notaria isso, certo?

Você realmente teria que encontrar uma vulnerabilidade nos ganchos do firewall no kernel do Linux que manipulam a pilha de rede ou apenas a pilha de rede antes de ser processada pelo firewall (?), ou o driver da placa de interface de rede se todas as conexões fossem negadas.

Vamos aos pontos de algumas experiências na vida real:

1)Se o computador se conectar a algo, o que ele conecta pode enviar dados que exploram vulnerabilidades no software que se conecta a ele, ou se for uma conexão não criptografada, alguém pode fazer um ataque Man In The Middle se os dados passarem pelo computador, onde eles recebem os dados corretos para serem enviados pela conexão, e enviam os dados errados para enganar ou atacar o computador ou o usuário.


2) Pelo que entendi, há uma "pilha de rede" no kernel do Linux que é analisada por código para filtrar pacotes de várias maneiras, que é programada por software de firewall como nftables ou iptables.

 
3) Acho que se houver algo errado com o driver da placa de interface de rede, você pode assumir o controle de um computador, não sei como funciona realmente, mas acho que os drivers provavelmente seriam restritos no que podem acessar, e teria que ser um código ruim que um pacote em si desencadearia a exploração. Isso parece que você precisaria de uma "tempestade perfeita" de vulnerabilidades. Então, há a pilha de rede em si, na qual os pacotes estão, que são analisados pelo firewall. Então, se houver uma vulnerabilidade na pilha de rede ou firewall de tal forma que APENAS enviar um pacote com algo errado com ele mexa com as coisas, sim, você pode ter um problema. Alguém que seja um desenvolvedor de kernel ou algo assim terá que verificar toda essa resposta.


4) Um site malicioso pode explorar uma vulnerabilidade encontrando uma fraqueza na forma como um navegador processa certos scripts e "se liberta" de sua sandbox, permitindo que ele assuma o controle do processo e faça o que puder dependendo da vulnerabilidade.


Agora vamos aos pontos do que normalmente é o termo técnico:

5) Se não houver serviços escutando conexões, então não há como um invasor remoto entrar pela rede. Você pode verificar isso com uma ferramenta como o nmap, se quiser ter certeza de que não está executando nenhum desses serviços.


6 ) nftables e iptables são implementados no kernel, eles rejeitam pacotes indesejados imediatamente.


7) também se deve verifique o roteador, ele pode ser mais vulnerável do que seu computador.


8) Mesmo que você não seja vulnerável a conexões de rede externas, você ainda é potencialmente vulnerável quando você mesmo inicia uma conexão com a internet. Mesmo que você não baixe e salve um arquivo, apenas visualizar um arquivo ou imagem requer algum código para executar e, ocasionalmente, no passado, houve vulnerabilidades que poderiam ter sido exploradas por invasores. Mesmo muito recentemente, essas vulnerabilidades foram encontradas em algumas bibliotecas de imagens (libwebp, libvpx).


Portanto, agora vamos a uma boa prática:

9) Usar um firewall e desabilitar quaisquer serviços de internet não utilizados.


10) Para qualquer software que você executar que se conecte à internet, execute-o em sandbox se possível (uma opção é usar flatpaks).


11) Não instale nada da internet. Use o gerenciador de pacotes nativo do seu sistema e repositórios quando possível, e para software de terceiros não disponível lá, use versões oficiais de fontes confiáveis quando possível, ou use flatpaks.


12) Execute atualizações diariamente para que você obtenha correções de segurança prontamente, quando vulnerabilidades sérias forem descobertas. 


13) Existe vulnerabilidades que são feitas a propósito pelo fabricante, tome cuidado com os navegadores que instala no computador, faça a instalação do navegador em uma conta de usuário com acesso restrito, os vírus não tem permissão para gravar, remova a permissão de escrita e gravação das unidades de discos, apesar de você possuir um firewall em sua rede interna, você deve instalar um firewall na sua máquina windows, um AD com politicas de acesso + proxy pode melhorar sua segurança de rede interna, mas não está descartado a possibilidade de um ataque.


14) Serviços de VPN e PROXY não garante sua segurança, pelo contrário do que muita gente pode pensar, você pode virar um vetor de ataque, pois na conexão da vpn você sai de dentro da sua rede, e entra em outra rede, e seu firewall fica inútil, sua conexão pode ser monitorada, e seus dados podem ser interceptados, sempre antes de contratar uma vpn, verifique se a empresa é confiável.


Bom agora vamos ao resumo geral:

Este firewall é completamente seguro?

Vamos supor que você gostaria de saber se há alguma falha/buraco nessa configuração de firewall. E que você queira bloquear o servidor o máximo possível para que seja impossível hackear. Os únicos serviços em execução serão openvpn e ssh.

Não. Nenhum firewall é completamente seguro. Enquanto houver uma única porta aberta, você estará vulnerável a ataques.

No entanto, é o mais seguro possível, dadas as circunstâncias.

É bom ver que você está executando o SSH em uma porta diferente da padrão.

Se o computador estiver conectado à Internet, ele não pode ser 100% inviolável. Se houver uma falha no OpenVPN, SSH, placa de rede ou processador, um invasor ainda poderá obter acesso por esses meios. E o firewall é tão forte quanto as senhas usadas nos protocolos que você passa por ele.

Certifique-se de ter senhas realmente fortes. Melhor ainda - não confie apenas em senhas, mas confie em uma combinação de senha e chave, então se alguém conseguir sua senha, isso não vai fazer muito bem a você. E talvez restrinja de onde você está permitindo que as conexões com SSH e OpenVPN venham - interrompa quaisquer conexões de lugares como China, Coreia do Norte, Iran, etc.

Você pode querer restringir os intervalos de ip, dos quais você pode alcançar ssh (e talvez openvpn). Certifique-se de adicionar pelo menos um intervalo, onde você sabe que terá acesso mesmo quando seu provedor troca a sub-rede da qual seu ip vem. Se você tiver outro servidor, adicione seu ip. Você pode até considerar permitir ssh somente de tal host.

O ponto é que você não tem segurança perfeita, porque um sistema é tão seguro quanto seu componente mais fraco. Se ssh, openvpn, processador e placa de rede puderem ser comprometidos, alguém pode invadir seu servidor por esses meios, e se houver algo errado no código iptables/nftables, você pode ter pacotes escapando. Além disso, se openSSH e/ou openVPN estiverem usando criptografia muito fraca, as conexões também podem ser atacadas.

Além disso, se você quiser melhorar ainda mais sua segurança, reconfigure o openSSH para usar autenticação de chave pública.

Bem é isso por enquanto.

Que tal pensar no futuro da segurança! Crie o hábito de procurar por falhas no kernel do Linux e do daemon do SSH. Previna-se, proteja-se.


Comece hoje mesmo dando uma olhada nessa falha do kernel:


https://nsfocusglobal.com/pt-br/linux-kernel-privilege-escalation-vulnerability-cve-2024-1086-notice/


Boas Fontes para manter-se informado:


https://www.exploit-db.com

https://thehackernews.com (Procure no Telegram: thehackernews)



