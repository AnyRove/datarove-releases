<p align="center"><img src="docs/images/icon.svg" width="96" alt="DataRove"></p>

<h1 align="center">DataRove</h1>

<p align="center"><b>Gerencie todos os seus bancos de dados no computador e acompanhe pelo celular, com criptografia de ponta a ponta.</b></p>

<p align="center">
<a href="README.md">English</a> · <a href="README.zh-CN.md">简体中文</a> · <a href="README.zh-TW.md">繁體中文</a> · <a href="README.ja.md">日本語</a> · <a href="README.ko.md">한국어</a> · <a href="README.es.md">Español</a> · <a href="README.fr.md">Français</a> · <a href="README.de.md">Deutsch</a> · <b>Português</b> · <a href="README.ru.md">Русский</a>
</p>

---

O DataRove é um cliente de banco de dados para desktop com backups agendados e sincronização embutidos. Ele roda no seu computador (como aplicativo ou serviço em segundo plano), e o celular se conecta pela rede local ou por um relay. A conexão é criptografada de ponta a ponta, e as senhas dos bancos nunca saem do computador.

**Bancos de dados:** MySQL · MariaDB · PostgreSQL · SQLite · SQL Server · MongoDB · Redis, com túneis SSH e TLS.

> **Status:** 1.0.0. Baixe os instaladores na página de [Releases](../../releases); depois disso, os apps se atualizam sozinhos. Clique em “Watch” neste repositório para ser avisado de novas versões. Mudanças no [CHANGELOG](CHANGELOG.md).

## Tour de 4 minutos em vídeo

<p align="center"><a href="docs/video/DataRove-promo.mp4"><img src="docs/video/DataRove-promo-poster.jpg" width="720" alt="Tour pelo DataRove (vídeo, narração em inglês, legendas em inglês e chinês)"></a></p>
<p align="center"><a href="docs/video/DataRove-promo.mp4">Assistir ao vídeo</a> (MP4, 1080p, narração em inglês com legendas em inglês e chinês) · legendas SRT: <a href="docs/video/DataRove-promo.en.srt">English</a>, <a href="docs/video/DataRove-promo.zh-CN.srt">简体中文</a></p>

![Visão geral](docs/images/overview.png)

## Recursos

### Navegar e editar dados
- Grade rápida com rolagem virtual, filtros, ordenação e paginação. Edite células no lugar ou abra uma linha inteira como formulário, com um campo adequado a cada tipo de coluna (booleano, enum, data, JSON, NULL / valor padrão).
- Em colunas de chave estrangeira, pesquise na tabela referenciada e escolha uma linha.
- Veja o SQL exato antes de salvar. Todas as alterações são confirmadas em uma única transação e revertidas juntas em caso de erro. Exclusões e conexões de produção pedem confirmação.
- Copiar como CSV / TSV / JSON / SQL / Markdown, exportar arquivos, importar CSV / JSON.

![Grade de dados](docs/images/data.png)

### Escrever consultas
- Editor SQL com autocompletar conforme o contexto: tabelas depois de `FROM` / `JOIN`, colunas depois de `alias.`, listas de colunas do `INSERT`, funções com assinatura e modelos de comandos.
- Passe o mouse sobre uma tabela ou coluna para ver estrutura, tipos e comentários. Nomes de tabela errados, `alias.coluna` inexistentes e `DELETE` / `UPDATE` sem `WHERE` são sublinhados antes de executar.
- O resultado de um `SELECT` em uma única tabela que inclua a chave primária pode ser editado e salvo diretamente.
- Sintaxe do shell do MongoDB (`db.users.find({...})`) e linha de comando do Redis, ambas com autocompletar.

![Editor de consultas](docs/images/query.png)

### Entender o esquema
- Bancos → tabelas / views / coleções → colunas, índices, chaves estrangeiras, triggers, DDL, número de linhas e tamanho.
- Diagrama ER, comparação de esquemas entre dois bancos com SQL de migração gerado, navegador de chaves do Redis, monitor do servidor (conexões, QPS, consultas lentas).

![Diagrama ER](docs/images/er.png)

### Importar e exportar
- Exporte uma tabela ou um resultado filtrado como CSV, TSV, JSON, JSON Lines, SQL `INSERT` ou Markdown; importe CSV / TSV / JSON / JSON Lines (UTF-8 e GBK são reconhecidos).
- Transferências grandes rodam em segundo plano no computador, com progresso ao vivo e botão de cancelar. Os arquivos vão em pedaços pequenos, então uma conexão perdida continua de onde parou.
- Uma importação roda em uma única transação: se uma linha falhar, nada é gravado e a linha com problema é indicada.

### Backups confiáveis
- Backup em arquivos com as ferramentas nativas (mysqldump / pg_dump / mongodump) ou o exportador embutido, compressão gzip e criptografia AES-256-GCM, mantendo os últimos N arquivos ou N dias.
- Cada novo arquivo de backup é lido de volta logo após ser gravado (checksum, descriptografia, análise), então um arquivo corrompido é descoberto na mesma noite.
- Sincronização com outro banco: substituição completa, esvaziar e carregar, acrescentar, upsert por chave primária ou sincronização **incremental** por uma coluna de marca d’água (`updated_at`, id autoincremento…).
- Agendamentos: a cada N minutos, diário, semanal, mensal ou cron. Execuções perdidas são feitas na inicialização. Notificações na área de trabalho e por webhook. Restaure qualquer backup em qualquer conexão com um clique.
- A sincronização também funciona entre tipos de banco (por exemplo PostgreSQL → MySQL). Cada tabela é gravada por completo ou não é tocada, e as contagens de linhas são conferidas antes do commit. Erros temporários são repetidos automaticamente; se algo ainda falhar, **repita só as partes que falharam**. Ferramentas que faltam (pg_dump etc.) são instaladas pelo app com um clique.

![Backups](docs/images/backups.png)

### Desfazer erros
- Em conexões marcadas como *produção*, o DataRove salva as tabelas afetadas antes de um `UPDATE`, `DELETE`, `DROP`, `TRUNCATE`, edição na grade, importação que sobrescreve, sincronização ou restauração.
- Desfaça depois com um clique: só as linhas diferentes voltam ao que eram, em uma transação. Os snapshots ficam criptografados no computador.

### Seu celular, com segurança
- A mesma interface no celular. Escaneie um QR code para parear, sem precisar de conta.
- Cada celular tem um papel (somente leitura, leitura e escrita ou administrador) que você pode mudar ou revogar a qualquer momento. Permissões temporárias expiram sozinhas, e um registro de atividades mostra o que cada celular fez.
- Na mesma rede Wi-Fi o celular se conecta direto; fora dela, passa por um relay que só repassa texto cifrado.

<p align="center"><img src="docs/images/phone-data.png" width="300" alt="DataRove no celular"></p>

### Desktop
Fica na bandeja do sistema, inicia com o login e pode se instalar como serviço em segundo plano (launchd / systemd / Agendador de Tarefas do Windows), para que os backups agendados continuem com a janela fechada. 10 temas, claro e escuro, 15 idiomas de interface.

## Segurança em resumo

- As senhas salvas são criptografadas no computador com uma chave mestra local. A interface e os celulares pareados só veem `••••`.
- O pareamento por QR code usa um segredo de 256 bits; o código curto digitado à mão é reforçado com scrypt. Os dois lados autenticam a chave um do outro, então um relay não consegue infiltrar um dispositivo próprio.
- Cada sessão usa X25519 + ChaCha20-Poly1305 com novas chaves efêmeras (sigilo futuro). Quadros adulterados, repetidos ou fora de ordem são rejeitados, assim como tentativas de rebaixar o protocolo.
- Um relay público precisa usar `wss://`. Você pode hospedar o próprio relay, e o pareamento pode ser feito só na rede local.
- Tudo o que vai além do próprio DataRove fica restrito ao computador: caminhos de ferramentas, navegação em arquivos, acesso a arquivos SQLite e envio de uma senha salva para um host alterado.

Para relatar uma vulnerabilidade, veja [SECURITY.md](SECURITY.md).

## Plataformas

| | |
|---|---|
| macOS | Apple silicon e Intel |
| Windows | 10 / 11, x64 |
| Linux | AppImage, deb |
| Android | App (companheiro de um computador, ou modo direto) |
| Celular | Qualquer navegador móvel atual (instalável como aplicativo web) |

## Instalação

Baixe o arquivo do seu sistema na [versão mais recente](../../releases/latest). Cada versão lista as somas SHA-256 em `SHA256SUMS.txt`. Os instaladores ainda não são assinados com um certificado de desenvolvedor da Apple ou da Microsoft, por isso o sistema pergunta uma vez:

- **macOS**: abra o `.dmg` e arraste o DataRove para Aplicativos. Se, na primeira abertura, o macOS disser que não consegue verificar o app, abra **Ajustes do Sistema → Privacidade e Segurança**, role para baixo e clique em **Abrir Mesmo Assim**. Se o macOS disser que o app está danificado, execute `xattr -cr /Applications/DataRove.app` no Terminal e abra de novo.
- **Windows**: execute o `.exe`. Se o SmartScreen aparecer, clique em **Mais informações → Executar assim mesmo**.
- **Linux**: torne o `.AppImage` executável e execute-o, ou instale o `.deb`.
- **Android**: abra o `.apk` e permita a instalação pelo navegador ou gerenciador de arquivos. O app é pareado com o DataRove no seu computador ou, no modo direto, conecta-se ele mesmo aos seus bancos de dados (ative o telefone uma vez com sua conta DataRove).

Depois, entre no app com seu e-mail e ative o computador. O DataRove é gratuito; não é preciso informar dados de pagamento.

## Licença, aviso legal e versões

- O DataRove é gratuito por enquanto, com todos os recursos. Não é preciso informar dados de pagamento.
- O DataRove é software proprietário, não de código aberto. O uso do app é regido pelo Contrato de Licença de Usuário Final, pelo Aviso de Privacidade e pelos Termos de Compra exibidos no primeiro uso, que você aceita no próprio app. Veja [LICENSE.md](LICENSE.md) (prevalece o texto em inglês).
- O DataRove altera bancos de dados reais. Mantenha backups independentes e testados e leia o [aviso legal](DISCLAIMER.md).
- As versões seguem o [Versionamento Semântico](https://semver.org/lang/pt-BR/). Cada versão lista suas mudanças em [CHANGELOG.md](CHANGELOG.md) e inclui um `SHA256SUMS.txt`.

## Feedback

Relatos de bugs e sugestões são bem-vindos em [Issues](../../issues). Este repositório guarda as notas de versão e os instaladores; o código-fonte não é público.
