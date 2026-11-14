# GIMP-Help

GIMP-Help é um sistema de ajuda criado para exibir o Manual do Usuário do GIMP
em um navegador ou em renderizadores HTML.  
Nossa documentação é escrita em [DocBook XML](https://docbook.org/), o que nos
permite criar um sistema de documentação altamente personalizável.

Atualmente mantemos as seguintes versões da documentação:
- O manual em desenvolvimento para a versão 3.2 do GIMP está disponível no
branch `master`. Aceitamos e revisaremos MRs destinados à 3.2.
- O manual para a versão 3.0 do GIMP está disponível em um branch chamado
`gimp-help-3-0` e é duvidoso que haja novas atualizações.


## Conteúdo

1. [Manuais online](#manuais-online)
2. [Dicas para contribuir](#dicas-para-contribuir)  
  2.1 [Traduzindo](#como-ajudar-a-traduzir-o-manual)  
  2.2 [Escrevendo o manual](#como-ajudar-a-escrever-o-manual)
3. [Problemas na documentação](#problemas-na-documentação)
4. [Criando uma versão](#criando-uma-versão)
5. [Atualizando os idiomas suportados](#atualizando-os-idiomas-suportados)
6. [História do módulo gimp-help](#história-do-módulo-gimp-help)


## Manuais online

Os manuais online, atualizados diariamente, para todos os idiomas suportados
estão disponíveis em:

- [Manuais do GIMP 3.2](https://docs.gimp.org/index.html#gimp-online) (build com Autotools)
- [Manuais do GIMP 3.2](https://testing.docs.gimp.org/index.html#gimp-online) (*build experimental com meson)

(Isso não é um erro de digitação.)

Os manuais de versões mais antigas do GIMP continuam disponíveis:

- [Manuais do GIMP](https://docs.gimp.org/online.html)

Os manuais offline podem ser baixados da nossa [página de versões](https://docs.gimp.org/download.html).

## Dicas para contribuir

### Como ajudar a traduzir o manual

Se você tem interesse em traduzir o manual, a melhor forma de
começar é ir até a [plataforma de tradução DamnedLies](https://l10n.gnome.org/).
Cadastre uma conta lá, caso ainda não tenha uma, e solicite entrada na equipe
de idioma do idioma que você quer traduzir.  

Todas as nossas traduções são feitas pelo DamnedLies. Para começar a traduzir o
manual, vá até os [arquivos po traduzíveis](https://l10n.gnome.org/module/gimp-help/)
do manual. Depois de terminar a tradução, você o envia ao DamnedLies.
Sua equipe de idioma poderá dar mais detalhes sobre esse procedimento.

**Nota:** se o seu idioma não tiver nenhum arquivo po listado lá, primeiro
é preciso adicioná-lo ao nosso repositório. Nesse caso, abra
[um issue](https://gitlab.gnome.org/GNOME/gimp-help/issues) informando qual
idioma e qual código de idioma iso você gostaria de ver incluído. Informe também
o nome nativo do idioma, que será usado no nosso site.

#### Como testar sua tradução

Se você já configurou um ambiente de build para o gimp-help, pode testar a
tradução:
- Ao usar o meson (preferencial):
  + Configure seu build do meson, caso ainda não tenha feito isso; veja
  [Instruções do build experimental com meson](#instruções-do-build-experimental-com-meson).
  + Execute `ninja && ninja install`. Se tudo estiver configurado corretamente,
  o GIMP deve encontrar o html atualizado ao abrir a ajuda.
  + Como alternativa, execute apenas `ninja`, navegue até a pasta de build do
  gimp-help e abra `html/LANG/index.html`, onde `LANG` deve ser substituído
  pelo código de idioma iso do seu idioma.
- Ao usar o autotools:
  + Supondo que seu build com autotools já esteja configurado, execute `make html-LANG`,
  onde LANG é o código de idioma iso. Isso gera uma versão HTML
  do manual do usuário. Por exemplo, use `make html-nl` para gerar o HTML
  do idioma neerlandês.
  + Quando isso funcionar, o arquivo index.html estará na pasta
  `/html/LANG/` dentro da sua pasta de build do gimp-help.

#### Como adicionar capturas de tela localizadas

As imagens localizadas também precisam ser revisadas antes pela sua equipe de
idioma pelo DamnedLies. Combine com a sua equipe de idioma como fazer isso;
após a revisão, elas precisam ser enviadas ao repositório do gimp-help por meio
de um Merge Request (MR).
Ou o líder da sua equipe de idioma abre o MR com as imagens localizadas, ou,
no mínimo, o líder da equipe precisa comentar no MR que as mudanças foram
conferidas e revisadas pela sua equipe.

Todas as imagens ficam na pasta `images`, na raiz do projeto. As capturas
originais em inglês estão na subpasta `C`. As capturas localizadas ficam em
pastas `LANG`, onde LANG é o código de idioma iso.

É importante preservar tanto os caminhos quanto os nomes ao salvar versões
localizadas das capturas de tela. Caso contrário, a documentação não será
compilada corretamente, ou as imagens localizadas não serão usadas.

De modo mais geral, siga também as [diretrizes](documentation-guidelines.md)
relevantes sobre o tratamento de imagens. Em geral, use o mesmo tema e o mesmo
tema de ícones da imagem em inglês.

### Como ajudar a escrever o manual

Se, em vez disso, você quiser ajudar a escrever e melhorar o
manual, então este é o lugar certo para começar.

Primeiro, seria uma boa ideia acompanhar as mensagens com as tags GIMP ou
documentation no [servidor Discourse do Gnome](https://discourse.gnome.org/).
Nota: ao fazer uma pergunta no Discourse, certifique-se de adicionar as tags GIMP
e documentation (ou i18n, se for sobre traduções), caso contrário podemos
não ver a sua mensagem.

A maioria dos desenvolvedores do GIMP também pode ser encontrada no canal
#gimp no IRC, em irc.gimp.org. As melhores chances de conversar são
normalmente no horário da noite europeia.

Se alguma das instruções abaixo não estiver clara, não hesite
em perguntar em um dos lugares mencionados acima.

#### Instruções do build experimental com meson
Os Makefiles do autotools estão sendo descontinuados e serão substituídos
pelo meson, que atualmente está em fase experimental. Você é
incentivado a usá-lo para compilar o manual. Isso deve
ser mais rápido do que compilar com autotools.
Para os pacotes necessários, veja abaixo; além deles, você também precisará
do pacote `meson`.

Para compilar com meson:
- Abra um terminal.
- Vá para o seu diretório de build (crie-o se necessário).
- Defina as variáveis de ambiente `$HELP_PREFIX` e `$SRCDIR`, ou substitua
pelos valores reais abaixo. Substitua "en nl" pelo(s) idioma(s)
que você quer compilar.
- `meson setup -Dprefix=$HELP_PREFIX $SRCDIR -Dlanguages="en nl"`
- Como alternativa, você pode iniciar o meson a partir do código-fonte e indicar
o diretório de build. Veja o manual do meson para detalhes.
- Em seguida execute: `ninja`. Ao compilar vários idiomas (digamos mais
de 4), você pode especificar uma carga máxima, por exemplo `ninja -l 4.5`.
- Se quiser, você pode instalar os idiomas para que o GIMP encontre
o manual local. Execute: `ninja install --no-rebuild`.
- Opcionalmente, teste se tudo está certo no build executando
`ninja test`.

*Continuando com as instruções normais*
Para começar, aqui está uma lista de verificação do que você deve fazer:
- Se você ainda não tem uma conta no [Gnome GitLab](https://gitlab.gnome.org/),
  crie uma (é possível entrar com credenciais do GitHub).
- Certifique-se de configurar uma chave [SSH](https://handbook.gnome.org/infrastructure/ssh-keys.html),
  caso contrário não conseguirá criar forks de projetos.
- Vá até https://gitlab.gnome.org/GNOME/gimp-help e
  crie seu próprio fork clicando em "Fork" no canto superior
  direito da página.
- Vá até o seu fork em https://gitlab.gnome.org/SEU_USUARIO/gimp-help
- O gimp-help usa um pipeline de CI para validar o conteúdo do repositório a cada
  commit. Certifique-se de configurar as opções de CI do seu fork corretamente:
  + `Settings > General > Visibility`:
    - Defina `Project visibility` como `Public`
    - Habilite `Repository` com `Everyone With Access`
    - Habilite `CI/CD` com `Everyone With Access`
    - Habilite `Container registry` (qualquer nível de visibilidade)
  + `Settings > CI/CD > General pipelines`:
    - Habilite `Public pipelines`
- Certifique-se de ler nossas [diretrizes de documentação](documentation-guidelines.md),
  que incluem o [guia de estilo](documentation-guidelines.md#style-guide).
- Para edições relativamente pequenas e simples, você pode usar o web-ide
  se não quiser configurar um ambiente de build no seu computador.
  + No menu à esquerda, expanda a seção `Code` e depois selecione
    `Repository`.
  + Na lista `+` no topo da página, selecione `New branch` e então
    crie um novo branch a partir de `master` para suas alterações.
  + De volta à página do Repository, no menu `Edit`, selecione `Web IDE`.
  + No web IDE, use o explorador de arquivos à esquerda para navegar até
    a pasta `/src/` e então selecione o arquivo que você quer atualizar.
  + Faça as mudanças necessárias.
  + No menu de ícones à esquerda da página, selecione
    `Source Control`.
  + No campo `Commit message`, digite uma breve descrição das suas mudanças.
  + Acima do campo `Commit message`, selecione o ícone de confirmação para
    fazer o commit das suas mudanças.
  + Certifique-se de que o pipeline de CI passa. Caso contrário, inspecione o
    pipeline para descobrir a causa da falha e corrija. Para ver o status do
    pipeline de CI, selecione `Build > Pipelines`.
  + Em seguida, você precisa criar um Merge Request (MR) para o commit.
  + Esse MR aparecerá no repositório principal do gimp-help e
    será revisado e avaliado.
- Para edições mais complicadas, reordenação de partes da ajuda,
  ou inclusão de capturas de tela, você deve clonar o repositório no
  seu computador e trabalhar nele localmente.
  + No seu fork do gimp-help, clique no botão azul `Code`
    e escolha o seu método preferido para clonar o repositório.
  + Caso você esteja no Windows, é fortemente recomendado
    configurar um ambiente MSYS2, para que você possa compilar e
    testar o repositório gimp-help. O MSYS2 inclui vários ambientes, cada
    um deles acessado pelo seu próprio executável. Recomendamos usar
    UCRT64 ou CLANG64. Para mais informações, consulte
    [Environments](https://www.msys2.org/docs/environments/) na documentação
    do MSYS2.
  + TODO: outros sistemas operacionais também podem precisar de alguma configuração. Adicione
    detalhes aqui conforme necessário.
  + Você precisa dos seguintes pacotes para compilar a documentação:
    + Mínimo: autotools ou meson [pelo menos versão 1.10], libxslt, python,
      perl, libxml2, python3-libxml2 (no MSYS2 isso está incluído no libxml2),
      docbook-xml e docbook-xsl.
      Com esses pacotes, você pode compilar a ajuda html em inglês.
    + Controle de versão: git.
    + Para compilar os PDFs de início rápido: librsvg2-2. Para alguns idiomas, pode ser necessário
      instalar as [fontes Noto](https://fonts.google.com/noto) apropriadas.
      Para mais informações, consulte
      [#410](https://gitlab.gnome.org/GNOME/gimp-help/-/issues/410)
      Para instalar o librsvg2-2 no MSYS2, consulte
      [Base Package: mingw-w64-librsvg](https://packages.msys2.org/base/mingw-w64-librsvg)
      para saber qual pacote instalar. Você pode usar `make pdf` para
      compilar os PDFs de início rápido de todos os idiomas.
    + Para localização: gettext, intltool e o pacote Python polib para
      validação dos arquivos po.
      Para instalar o polib, use um gerenciador de pacotes (por exemplo, pacman) para instalar o pip,
      e depois use o pip para instalar o polib. O gettext pode já estar instalado
      em um ambiente MSYS2.
    + Para compilar os instaladores do Windows: intltool e innosetup. Ao usar o meson,
      o InnoSetup será baixado e instalado automaticamente quando necessário.
    + Para criar um diagrama de arquitetura do processo de build: dot, que faz
      parte do graphviz. Ainda não disponível no nosso build com meson. Para o MSYS2, consulte
      [Base Package: mingw-w64-graphviz](https://packages.msys2.org/base/mingw-w64-graphviz)
      para saber qual pacote instalar. Para gerar o diagrama,
      use `make dot`.
    + Para compilar versões em PDF da ajuda: dblatex (não funciona no momento).
      Atualmente não há plano de portar isso para o meson. Observe que as versões em PDF
      do gimp-help não são mais criadas, porque o dblatex parece
      ser incompatível com versões mais recentes do Python. Consulte
      [#138](https://gitlab.gnome.org/GNOME/gimp-help/-/issues/138) para mais
      informações. Quando tudo estiver configurado corretamente, use `make pdf-xx` para
      compilar um PDF de um único idioma ou `make pdf-local` para compilar PDFs de
      todos os idiomas.
      + TODO: testar e confirmar o processo de instalação do dblatex. Para mais
        informações, consulte o
        [site do dblatex no SourceForce](https://dblatex.sourceforge.net/).
    + Nota: o script autogen também verifica se pngnq e pngcrush estão
      instalados, mas eles não são usados.
  + Supondo que você clonou o repositório git no seu computador,
    crie um diretório de build dentro ou fora da árvore, conforme
    sua preferência pessoal, e então execute a partir do diretório de build:
    `../autogen.sh --prefix=${INSTALL_PREFIX} --without-gimp`.
    + Nota: se você quiser compilar a ajuda apenas para um subconjunto específico dos idiomas
      suportados, adicione `ALL_LINGUAS="en xx"`, onde o valor entre aspas é uma
      lista de códigos de idioma separados por espaço. Assim, quando você usar um comando `make`
      sem especificar um idioma, como `make html` em vez de
      `make html-en`, você compilará a saída apenas para os idiomas escolhidos.
    + Nota: você pode executar o script autogen novamente para regenerar os makefiles. Por
      exemplo, se quiser mudar para quais idiomas você compila a ajuda, ou
      se instalar um novo pacote.
  + TODO: no Windows talvez você precise adicionar `--build=mingw64`,
    possivelmente também `DESTDIR=$BUILD_DIR $SRCDIR`.
  + Depois de executar o autogen uma vez, você pode usar o comando a seguir
    repetidamente para gerar o manual em inglês:
    `make html-en`.
  + Você pode verificar a saída no seu diretório de build, na
    pasta /html/en/.
  + Se isso funcionar, selecione um arquivo da pasta /src/
    no qual você quer trabalhar e comece a editar.
  + Ao terminar um conjunto de edições relacionadas, você
    deve primeiro validar suas mudanças para verificar que
    você não cometeu erros no formato docbook XML.
    O comando para isso é `make validate-en`.
  + Se tudo estiver certo, faça o commit das suas mudanças com uma
    mensagem de commit descritiva. Certifique-se de sempre fazer
    mudanças relacionadas em um branch separado.
  + Envie suas mudanças para o seu fork no Gnome GitLab.
  + Certifique-se de que o pipeline de CI passa. Caso contrário, inspecione o pipeline para
    descobrir a causa da falha e corrija.
  + Vá até o seu fork online e clique no botão Merge Request.
- Certifique-se de ter as notificações habilitadas, porque você
  pode ser solicitado a mudar certas coisas, ou precisar explicar
  por que fez certas mudanças.
- Não espere uma resposta imediata. Há apenas uma pequena
  equipe trabalhando nisso. Uma revisão às vezes pode levar semanas ou
  até meses, embora tentemos responder o quanto antes.
- Algumas informações adicionais também podem ser lidas no manual, veja
  https://docs.gimp.org/en/gimp-contributing.html
  (embora algumas partes de lá precisem ser revisadas).

TODO
- Mencionar que o make às vezes faz com que certas traduções po
  sejam atualizadas. Elas não devem ser incluídas no commit junto com
  mudanças na documentação-fonte.
- Pode ser útil documentar o uso de ambientes virtuais para python
  (ou seja, [venv](https://docs.python.org/3/library/venv.html)). Com o MSYS2,
  um usuário pode encontrar o erro externally-managed-environment ao
  tentar instalar pacotes python pelo pip.

De preferência, compile você mesmo o manual mais atualizado ou confira
a última versão enviada em https://docs.gimp.org/en/.

O código-fonte do manual está em arquivos XML no diretório /src
deste repositório. Encontre um tópico que lhe interesse
ou que precise de atualização e comece a escrever.

Você também pode dar uma olhada no nosso issue tracker em
https://gitlab.gnome.org/GNOME/gimp-help/issues

  ### O que você deve saber

  Você deve saber um pouco de Docbook e XML, ou ser esperto o suficiente para aprender
  a sintaxe sozinho. Você pode obter mais informações sobre Docbook e XML
  usando o mecanismo de busca de sua preferência.
  Certifique-se também de ler nossas [diretrizes de documentação](documentation-guidelines.md).

  ### Editores, programas e configurações

  Use qualquer editor que dê suporte a editar XML. Tenha em mente que a
  largura do tab no modo XML deve ser de 2 espaços.

  Desde que você tenha o xmllint instalado, você pode validar o XML
  e verificar a boa formação dos arquivos XML executando

    make validate-en

  Também é possível executar `make validate`, mas validar todos
  os idiomas leva bastante tempo.
  Os tradutores são incentivados a validar o seu idioma substituindo
  `en` acima pelo código de idioma deles.

  Quando você edita um arquivo XML e quer verificar rapidamente suas mudanças,
  você pode criar um único arquivo HTML de rascunho, sem grandes preocupações, com

    make src/of/the/xml-file.draft

  onde o alvo é o caminho do arquivo com a extensão ".draft" em vez
  de ".xml", ou com

    make preview-src/of/the/xml-file.xml

  onde o caminho do arquivo é precedido por "preview-".

  O nome do arquivo HTML de rascunho depende do id dele (não do nome
  do arquivo XML de origem!) e é exibido quando o arquivo é criado.


###  Dicas para fazer boas capturas de tela

  * Ao fazer capturas de tela, certifique-se de usar o tema padrão do GIMP
    e os ícones Symbolic. Na maioria dos lugares usamos a versão escura do nosso tema,
    procure seguir isso.
    Se quiséssemos um visual completamente unificado, seria preciso criar todas as imagens
    em uma única plataforma com um tema escolhido. Isso excluiria colaboradores
    de outras plataformas, o que não é o que queremos.
  * Use fontes padrão como Bitstream Vera Sans - FIXME: esta não é uma
    fonte padrão multiplataforma, precisamos de uma sugestão melhor.
  * Corte as bordas do gerenciador de janelas; ou até mais, se a imagem for destinada
    a mostrar apenas um conjunto limitado de coisas que precisam de explicação.
  * Antes de salvar uma imagem como PNG, verifique se é possível convertê-la para o modo
    indexado sem perda de qualidade (economiza espaço e banda) FIXME:
    veja o pngnq, que faz isso para você, incluindo a redução de tamanho, mas cuidado:
    em imagens pequenas o tamanho pode aumentar. Isso também só funciona bem com
    capturas de tela que não mostram fotos ou coisas semelhantes. Você precisa verificar
    primeiro se a imagem resultante não difere muito da original.
    Talvez diga para não adicionar metadados a menos que seja necessário, sem comentário, sem miniatura,
    talvez também sem perfil de cor, ou seria melhor incluir? 
  * TODO: adicionar informações sobre os diferentes programas pngcrush
    pngnq, nos meus testes, costuma dar o menor resultado (configurações padrão), mas verifique
    se o resultado não é muito diferente do original.
  * Forneça suas imagens de origem (por exemplo, para fazer novas capturas de tela em outros
    idiomas) - adicione informações sobre onde, na árvore de código, elas devem ser colocadas,
    informações para manter o tamanho do xcf o menor possível, dimensões de imagem pequenas
    quando possível (64 x 64?) - marque "usar compressão melhor, porém mais lenta"
    ao salvar em xcf; para a maioria das imagens de exemplo, usar precisão inteira de 8 bits
    é suficiente, a menos que se queira demonstrar especificamente maior profundidade de bits.


## Problemas na documentação

Veja nosso issue tracker no Gnome Gitlab:

  https://gitlab.gnome.org/GNOME/gimp-help/issues


## Criando uma versão

**Nota:** esta seção precisa ser atualizada.

**Nota2:** para make dist e make dist-check é necessário automake 1.16.4 ou mais recente.
Versões mais antigas não reconhecerão README.md como uma versão válida
no lugar de README.

Antes de criar uma versão, você vai precisar:

    * ser um mantenedor
    * ter acesso ssh a pentagon.gimp.org
    * ter acesso a http://www.gimp.org/admin/
    * FIXME: verificar se isso ainda está atualizado.

### Passos

* Certifique-se de que todo o XML é válido. Execute:

    make validate-all

* Prepare o arquivo NEWS, adicionando uma nova versão, bugs corrigidos e
  colaboradores. Você pode usar um pequeno atalho para compilar os
  colaboradores usando o git shortlog (<release tag> é a tag da última
  versão):

    git shortlog -sn <release tag>..HEAD

* Nota: o comando acima nem sempre fica completo para tradutores, já que a maioria
  das traduções é enviada pelos coordenadores das equipes de idioma, não necessariamente
  pelas pessoas que fizeram a tradução de fato.

* Verifique se o authors.xml precisa ser ajustado para esta versão.

* Aumente o número da versão (help_(major, minor, micro)_version) no
  configure.ac, faça o commit, envie. Regra geral: ele deve documentar
  a versão estável atual do GIMP. A versão menor, portanto, acompanha
  a versão estável atual.

    vi configure.ac

* Crie um pacote de distribuição:

    make dist

* Marque a versão com uma tag:

    git tag -s

  Regra geral: use maiúsculas, separadas por sublinhado.

* Copie o *.bz2 para pentagon.gimp.org:/srv/ftp/pub/gimp/help/:

    scp gimp-help-*.tar.* pentagon.gimp.org:/srv/ftp/pub/gimp/help/

  Verifique se o tarball aparece em:

    http://download.gimp.org/pub/gimp/help/

* Adicione os instaladores do Windows

* Atualize nossos sites: docs.gimp.org e gimp.org/docs

* Anuncie a versão em http://www.gimp.org/admin/. Clique em `Pending
  News` → `New News`, preencha o formulário (subject, announce), escolha um
  wilber de leitura e pressione `Save`. Dependendo de precisar de revisão (pergunte a
  um dos desenvolvedores), aprove para publicá-lo.

* Anuncie a versão no Discourse usando as tags gimp e documentation:

    Discourse
    https://discourse.gnome.org/

  Modelo:

      Manual do GIMP <version> lançado

      Lançamos uma nova versão do manual do usuário com:

        * <Changelog here>

      Baixe os pacotes no nosso software de download.

    Para facilitar a instalação, sugerimos que você espere até que um instalador desta
    versão tenha sido empacotado para a sua plataforma. Encontre mais versões e informações
    sobre nossos objetivos e como você pode ajudar em https://docs.gimp.org.


## Atualizando os idiomas suportados

Ao adicionar um novo idioma para tradução, vários arquivos precisam ser
atualizados. Depois que o build com meson for declarado estável, os arquivos
relacionados ao autotools serão removidos. Eles estão marcados como *somente autotools*.

Um script Python está disponível para automatizar a atualização de todos os arquivos. Esse script
deve ser usado em vez de atualizar manualmente:
  - A partir da pasta de código-fonte do `gimp-help`, inicie `../tools/add_lang.py` com
  os seguintes parâmetros de linha de comando, nesta ordem:
    + código de idioma iso
    + nome do idioma em inglês (EUA)
    + nome nativo localizado do idioma
    + local da pasta de build (necessário apenas na segunda execução)
  - Preste atenção a possíveis avisos no terminal e, se necessário,
  aja de acordo com eles.
  - Execute o comando `meson` mostrado no final, seguido de `ninja`, que
    criará os arquivos po necessários, os quais precisam ser adicionados aos nossos
    arquivos meson.build.
  - Execute o script `add_lang.py` novamente com os mesmos parâmetros.
  - A menos que apareçam erros ou avisos no seu terminal, você já pode
    criar um MR com todos os arquivos alterados.

Caso haja problemas com o script, aqui está a lista de arquivos
que precisam ser atualizados:

  - [configure.ac](configure.ac); *somente autotools*
    - Atualize ALL_LINGUAS
  - [Makefile.GNU](Makefile.GNU); *somente autotools*
    - Atualize ALL_LINGUAS
  - [po/LINGUAS](po/LINGUAS) *somente meson*
    - Adicione o código do idioma em uma linha separada, na posição
    alfabética correta.
  - [quickreference/po/meson.build](quickreference/po/meson.build) *somente meson*
    Adicione o novo arquivo po em `qr_po_files`.
  - [po-windows-installer/meson.build](po-windows-installer/meson.build) *somente meson*
    Adicione o novo arquivo po em `installer_po_files`.
  - [stylesheets/languageVocab.xml](stylesheets/languageVocab.xml)
    - Atualize o vocab
  - [stylesheets/authors_common.xsl](stylesheets/authors_common.xsl)
    - Atualize `<xsl:variable name="languages">`
  - [tools/get_po_status.pl](tools/get_po_status.pl)
    - Atualize my %Languages
  - [web/website.xml](web/website.xml)
    - Atualize a lista de manuais do usuário online e PDFs de referência rápida
  - [.gitlab-ci.yml](.gitlab-ci.yml)
    - Adicione o código do idioma ao `MAKE_LANGS_` [número] correspondente. *somente autotools*
    - Adicione o código do idioma ao `MESON_LANGS_` [número] correspondente. *somente meson*
    - Nota: após o commit, certifique-se de que o job de build não leve
      muito tempo. Quando levar mais de 50 minutos, pode ser hora de dividir
      o job de build, porque não queremos time-outs.
  - [build/windows/installer/installer-gimp-help-msys2.sh](build/windows/installer/installer-gimp-help-msys2.sh) *somente autotools* **TODO** não é tratado pelo `add_lang.py`!
    - Verifique se o idioma faz parte dos idiomas oficiais instalados
      junto com o Inno Setup. Uma lista atualizada pode ser encontrada no
      [github](https://github.com/jrsoftware/issrc/tree/main/Files/Languages).
    - Se não, verifique se existe uma tradução não oficial,
      [aqui](https://github.com/jrsoftware/issrc/tree/main/Files/Languages/Unofficial).
    - Se existir, adicione-a aos comandos download_lang.
    - Se não houver tradução disponível, `Default.isl` pode ser usado.
  - [build/windows/installer/languages.isi](build/windows/installer/languages.isi)
    - Todos os idiomas com traduções do InnoSetup já estão adicionados. Você só
    precisa atualizar isso quando o InnoSetup adicionar um novo idioma ou quando o
    idioma que você está adicionando não tiver uma tradução do InnoSetup. Nesse
    caso, você precisa renomear uma cópia de `Default.isl` e atualizar o nome
    e o código do idioma. Depois:
    - Adicione um teste if LANG e defina LANGFILE para o seu idioma.
  - `po/novo código de idioma` precisa de arquivos `meson.build` copiados de outro idioma
    para todas as pastas. *somente meson*

*somente autotools*
Nota: se o seu diretório de build não for filho do diretório de origem,
o `msginit` não preencherá PACKAGE_VERSION no cabeçalho dos arquivos po.
Como o msginit tenta encontrar o `configure`, copiá-lo para o diretório de build
pode resolver esse problema. Nesse caso, certifique-se de atualizá-lo ao trocar
de branch.

Quando tudo acima estiver atualizado, execute:
  - `make po` no diretório de build principal
  - `make po` no diretório quickreference
    (cria arquivos po para a referência rápida)
  - `make update-po` no diretório po-windows-installer
    (cria arquivos po para o instalador do Windows; certifique-se de que o charset seja UTF-8)

Isso deve criar os arquivos po necessários para o idioma recém-adicionado.
Nota: alguns arquivos po de outros idiomas também podem ter sido atualizados, mas você
deve enviar apenas os arquivos relevantes para o novo idioma.


## História do módulo gimp-help

O desenvolvimento dos módulos gimp-help originais praticamente parou
depois das primeiras versões estáveis do GIMP 1.2 serem lançadas. Isso se deve
a vários motivos, um deles sendo que toda a documentação original havia sido
convertida de HTML para DocBook/SGML e, além de um
pouco de conteúdo novo, muita marcação e revisão de texto, não aconteceu muita coisa
na organização da bagunça completa.

Daniel Egger e Mel Boyce não ficaram muito felizes com as esquisitices
desse sistema de ajuda. Então eles começaram completamente do zero, criando um novo
manual baseado em Docbook/XML, chamado gimp-help-2.

Mais tarde, ele foi renomeado para gimp-help.
