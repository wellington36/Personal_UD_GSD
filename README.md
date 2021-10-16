# Personal UD GSD

Repositório de busca de erros no corpus [UD_Portuguese-GSD](https://github.com/UniversalDependencies/UD_Portuguese-GSD/tree/master).

## Descrição
 Rodamos no corpus do português GSD o [udpipe1.2](https://ufal.mff.cuni.cz/udpipe/1/users-manual) com um modelo treinado no [último commit](https://github.com/UniversalDependencies/UD_Portuguese-Bosque/commit/797b72ff7397d912d29285896314f661c16464ee) (até o momento) do corpus [UD_Portuguese-Bosque](https://github.com/UniversalDependencies/UD_Portuguese-Bosque) e armazenamos os novos conllu junto com as respectivas sentenças neste repositório para busca de inconsistências no corpus.
 
## Criação dos arquivos (Lembrete: caso queira rodar ajuste a posição dos arquivos para a sua maquina)
 - treinamento do modelo no Bosque:
    ```
    % ./udpipe --train bosque-udpipe-last-commit.udpipe ../../universal_dependencies/UD_Portuguese-Bosque/pt_bosque-ud-train.conllu
    ```
 
 - geração dos arquivos ```.txt``` com as sentenças (em python):
    ```python
    import pyconll
    import sys

    filearg = sys.argv[1]    # input file
    conllu = pyconll.load_from_file(filearg)    # parser

    sentences = []
 
    # captura das sentenças
    for sentence in conllu:
        sentences.append(sentence.text)
    
    # output
    for sent in sentences:
        print(sent)
    ```
 - retraino dos arquivos conllu do GSD:
    ```
    % for f in ../../Personal_UD_GSD/*.txt ; do ./udpipe --outfile=../../Personal_UD_GSD/$(basename $f .txt).conllu --tokenizer="normalized_spaces" --tag --parse ../../Personal_UD_GSD/bosque-udpipe-last-commit.udpipe $f; done
    ```
