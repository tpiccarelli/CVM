Instruções gerais CVM:
http://sistemas.cvm.gov.br/Port/DownloadArqs/download02.htm

1. Estrutura endereço:
http://www.rad.cvm.gov.br/ENETCONSULTA/frmDownloadDocumento.aspx?CodigoInstituicao=1&NumeroSequencialDocumento=NNNNN

2. Estrutura nome arquivo .zip:
XXXXXXAAAAMMDDYNN.zip
    onde:
    XXXXXX = ccvm
    AAAAMMDD = DataRef
    Y = tipo doc
        TODOS => Documentos e formulários recebidos pela CVM <1>;
        RAD => Os formulários ITR, DFP e IAN recebidos pela CVM <2>;
        ITR => Informações Trimestrais recebidas pela CVM <3>;
        DFP => Demonstrativos Financeiros recebidos pela CVM <4>;
        IAN => Informações Anuais recebidas pela CVM <5>;
        IPE => Informações Periódicas recebidas pela CVM <6>;
        ENET => Formulários do Empresas.Net recebidos pela CVM <7>.
    NN = 01

3. Conteúdo dos arquivos:
    3.1 ITR:
        *.itr
        *.fca
        FormularioCadastral.xml
        FormularioDemonstracaoFinanceiraITR.xml
    3.2 DFP:
        *.dca
        *.fca
        FormularioCadastral.xml
        FormularioDemonstracaoFinanceiraDFP.xml

# Escreve a função:

cvm <- function(n){
# obs.: n ~ 46000 (em 2014)

# Carrega packages:
require()
require()
require(XML)

# Define URL para download dos arquivos *.zp contendo o material em análise, sendo "n" o nº de registro do documento no sistema da CVM:
URL <- paste0("http://www.rad.cvm.gov.br/ENETCONSULTA/frmDownloadDocumento.aspx?CodigoInstituicao=1&NumeroSequencialDocumento=", n)

# Acessa o site de onde o arquivo *.zip é baixado automaticamente:
browseURL(url = URL)

# Lista arquivos *.zip disponíveis na pasta raiz e nomeia variável para extração e tratamento do seu conteúdo:
nome_zip <- list.files(pattern = "*.zip)

# Descompacta conteúdo baixado:
unzip(nome_zip)

# Remove arquivo "*.zip":
file.remove(nome_zip)

# Renomeia extensão do arquivo "*.itr" ou "*.dfp" para "*.zip":
file.rename(from = list.files(pattern = ".itr"), to = nome_zip)
file.rename(from = list.files(pattern = ".dfp"), to = nome_zip)

# Descompacta conteúdo da nova pasta:
unzip(nome_zip)

# Extrai infos gerais dos dados baixados
ccvm <- substr(x = nome_zip, start = 1, stop = 6)
DataRef <- substr(x = nome_zip, start = 7, stop = 14)
tipo_doc <- substr(x = nome_zip, start = 15, stop = 15)

# Transfere dados para data frame:
assign(x = ccvm, value = xmlToDataFrame("InfoFinaDFin.xml"))

 # Remove arquivos intermediários não utilizados:
file.remove(list.files(pattern = ".xml"))
file.remove(list.files(pattern = ".zip"))
file.remove(list.files(pattern = ".fca"))

... CONTINUA!
}
