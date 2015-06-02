cvm_read.xml_teste <- function(file){

    # Ler o arquivo .xml:
    cvm_xml <- xmlTreeParse(file)
    
    # Gerar o nó XML:
    raiz <- xmlRoot(x = cvm_xml)
    
    # Medir o comprimento dos dados XML:
    linhas <- length(raiz)
    
    # Criar vetores para geração do data.frame:
    contas <- array(data=0, dim = linhas)
    valores_BP <- array(data=0, dim = linhas)
    valores_DRE <- array(data=0, dim = linhas)
    
    # Estrutura:
    ## raiz [[x]][[y]][[z]]
    ## onde:
    ### [[x]]: nome da conta (x=1:length(raiz))
    #### xmlName(raiz[[1]][[n]]): header da coluna (n=1:18)
    ##### n=4: DescricaoConta1
    ##### n=8: ValorConta2
    ##### n=9: ValorConta3
    ##### n=14: ValorConta8
    ##### n=15: ValorConta9
    ##### n=18: ValorConta12
    ### [[y]]: Valor (4:DescricaoConta1; 8:ValorConta2; etc)
    ### [[z]]: restrição para visualizar somente o elemento do código XML (z=1)
    
    # Preencher valores nos vetores:
    for(i in 1:10){
        contas[i] <- xmlValue(raiz[[i]][[4]][[1]])
        valores_BP[i] <- as.numeric(xmlValue(raiz[[i]][[9]][[1]]))
        valores_DRE[i] <- as.numeric(xmlValue(raiz[[i]][[15]][[1]]))
    }
    
    # Criar data.table:
    arquivo <<- data.table(DescricaoConta1=contas,Valor3=valores_BP, Valor9=valores_DRE)
}
