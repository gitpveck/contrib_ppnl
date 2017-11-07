# contrib_ppnl

Contributions to the PPNL server infra. 

# Server migration roadmap

Servers in gebruik(2017) :

- mail.piratenpartij.nl
- piratenpartij.nl
- proxy.piratenpartij.nl

Dit gefaseerd migreren naar een geconsolideerd platform bestaande uit 1 KVM/VM -> LXC en Docker containers

**Pros**

- Alles hosted op een single server 
- kostenbesparend

**Cons**

- Geen resilience (server down = alles van PPNL down)
- Performance ?


##  Concept nieuwe KVM/VM server setup

### Architectuur

OS: Ubuntu16.x.LTS

Networking: NAT -> docker bridge 
                -> lxc bridge

LXC/LXD:  LXD init op ZFS 

Docker: applicatie containers scheiden van data. data in data volumes

richtlijn: 1 applicatie per container

 App | container type | code | comment
--- | --- | --- | ---
git| LXC | gitolite | central git repo voor onder andere deze README ;-) en ansible config management ?
mail|docker| [this](https://github.com/hardware/mailserver) or [this](https://www.mailcow.email/)| Het betreft 2 docker based setups die vergelijkbaar zijn met de huidige mailserver setup qua functionaliteit
http server| LXC| nginx | main server voor piratenpartij.nl virtual hosts. 
monitoring| LXC | nagios/chk_mk/munin | monitor all containers
maillijsten | LXC | mailman3 | 
LDAP | LXC | openldap |
Ticketsystem| LXC | OTRS | 
voice chat| LXC | mumble | 

De meeste oplossingen passen zowel in docker als in LXC 
Waar gaat de voorkeur naar uit ? Dit zal gezamenlijk besloten moeten worden..
