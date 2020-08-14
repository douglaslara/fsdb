# fsdb

Olha essa. Pra casa cliente eu mantenho um arquivo que só ele tenha direito de escrita, mas os outros clientes todos vão ficar com ele pra leitura. Toda vez que uma nova escrita precisa ser feita, ela é feita no arquivo de quem tá escrevendo. Se é um update ou delete, além de escrever no dele eu uso um optimistic locking, checando se nenhuma versão com mesmo sequencial foi criado por outros clientes nos arquivos deles. Toda leitura eh como um event sourcing so que usando todos os arquivos pra fazer scan. Tipo database partition, mas as partições são por cliente. Aí evito problema de criar locking pra mais de um querer escrever no mesmo arquivo.

Posso fazer um semaforo pra garantir que só um escreva ao mesmo tempo ou tipo um 2 phase commit, mas só pra garantir que não aconteca duplicação. O processo nunca fica em lock.

De tempo em tempo, eu consolido as entradas num único aquivo, que é o Golden Source, pra evitar a proliferação de um monte de Arquivo. Os arquivos de escrita aí ficam rápidos pra fazer refresh pra verificar esse delta de escritas.

arquivos centralizados pra não precisar fazer discovery.

Lê tudo que tá na raiz e pronto.
