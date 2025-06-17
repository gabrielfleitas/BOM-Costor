# BOM-Costor
BOM Costor: Otimização de Custos de Lista de Materiais (BOM)
O BOM Costor é uma ferramenta poderosa desenvolvida em Python com uma interface de utilizador simples e intuitiva, construída com Streamlit. O seu objetivo principal é ajudar empresas a consolidar Listas de Materiais (BOMs), gerir cotações de fornecedores, calcular os custos mais otimizados por cenário de produção e gerar relatórios analíticos abrangentes.

🚀 Funcionalidades Principais
Importação Flexível de BOMs:

Carregamento múltiplo de planilhas Excel (drag-and-drop).

Mapeamento de colunas personalizável (Part Number, Descrição, Fabricante, Quantidade por Montagem).

Definição e gestão de cenários de volume de produção e fatores multiplicativos.

Armazenamento de dados em SQLite com versionamento.

Consolidação Inteligente:

Normalização automática de Part Numbers (remoção de espaços, hífens, maiúsculas).

Utilização de Fuzzy-matching (RapidFuzz) para agrupar componentes idênticos, mas com grafias diferentes.

Cálculo da Quantidade Consolidada (Σ (qty per assy × lote)).

Gestão de Cotações:

Geração de templates de planilha Excel para cotação, prontos para serem enviados a fornecedores.

Importação de cotações preenchidas, com remapeamento de colunas de preço.

Funcionalidade de "copiar preço de outro cenário" (price-break) para otimizar a entrada de dados.

Cálculo de Custo e Ranking:

Conversão automática de moedas com taxas de câmbio editáveis pelo utilizador.

Ranking de preços por componente e fornecedor.

Cálculo do Custo Total da BOM por cenário, sempre com base no preço mais vantajoso.

Relatórios e Análises Visuais:

Tabelas interativas para o ranking de preços.

Gráficos de pizza para o share de custo por fornecedor.

Análise de Curva ABC dos componentes, identificando os itens de maior impacto no custo.

Robustez e Diagnóstico:

Validação de esquema de planilhas usando Pydantic.

Sistema de logging detalhado para rastreamento de erros e avisos.

Relatório de diagnóstico do sistema.

⚙️ Configuração e Instalação
Pré-requisitos
Python 3.9+: Verifique a sua versão com python --version.

Poetry: Ferramenta de gestão de dependências. Se não tiver instalado, siga as instruções em Poetry Installation.

Instalação do Projeto
Clone o Repositório (Assumindo que este código está num repositório Git):

git clone <URL_DO_SEU_REPOSITORIO>
cd bom_costor

Instale as Dependências com Poetry:

poetry install

Isso criará um ambiente virtual e instalará todas as dependências listadas no pyproject.toml.

Ative o Ambiente Virtual:

poetry shell

Como Executar a Aplicação
Com o ambiente virtual ativado (passo 3 acima), execute o seguinte comando:

streamlit run bom_costor/ui/app.py

Isso abrirá a aplicação no seu navegador web padrão (geralmente em http://localhost:8501).

👨‍💻 Tutorial de Utilização
Ao iniciar o BOM Costor, você verá uma barra lateral (sidebar) e a área principal da aplicação.

1. Gestão de Projetos (Sidebar)
Selecionar Projeto Existente: Se já tiver projetos, escolha um na caixa de seleção.

Criar Novo Projeto: Digite um nome para o novo projeto e clique em "Criar Novo Projeto". Todos os seus dados serão organizados por projeto.

2. Importação de BOMs (Página "Importar BOMs")
Selecione o Projeto: Certifique-se de que o projeto desejado esteja selecionado na barra lateral.

Carregar Arquivos: Arraste e solte suas planilhas Excel (até 20) na área designada ou clique para selecionar.

Configurar Cenários:

Cenários Atuais: Se já houver cenários para o seu projeto, eles serão exibidos. Pode editar o "Volume" e o "Multiplicador" de cenários existentes.

Novos Cenários: Adicione novos cenários definindo "Nome Cenário", "Volume" (peças) e "Multiplicador" (fator para Qty per assy × lote).

Mapeamento de Colunas:

O sistema tentará sugerir colunas com base no seu primeiro arquivo carregado.

Mapeie as colunas do seu Excel (Part Number, Descrição, Fabricante, Qty per assy) para os campos do sistema. "Part Number" e "Qty per assy" são obrigatórios.

Taxas de Câmbio Iniciais (Opcional): Pode definir pares de moedas e suas taxas de conversão (ex: USD para BRL). Estas taxas serão usadas nos relatórios.

Processar Importação: Clique em "Processar Importação de BOM(s)". O sistema importará os dados, normalizará os Part Numbers (eliminando variações como espaços/hífens) e consolidará as quantidades automaticamente.

3. Gestão de Taxas de Câmbio (Página "Gerir Taxas de Câmbio")
Ver Taxas Atuais: Visualize todas as taxas de câmbio configuradas para o projeto atual.

Adicionar/Atualizar Taxa: Insira as moedas de origem e destino (ex: USD, BRL) e a taxa de conversão (ex: 5.20 para 1 USD = 5.20 BRL). Clique em "Salvar Taxa de Câmbio" para adicionar ou atualizar.

4. Gerenciamento de Cotações (Página "Gerir Cotações")
4.1. Gerar Template de Cotação
Defina o "Número de Colunas de Fornecedores" (até 11: 10 fornecedores + 1 em branco).

Clique em "Gerar Template Excel para Cotação".

Baixe o arquivo gerado (template_cotacao_<nome_do_projeto>.xlsx).

Preencha o template: Insira os preços unitários cotados pelos seus fornecedores para cada componente e cenário. Pode adicionar uma coluna extra com a moeda da cotação (ex: Volume 100 • $ Forn1 (Moeda)). Para replicar um preço de um cenário de menor volume (price-break), digite copiar na célula do preço.

4.2. Importar Cotações Preenchidas
Carregue o arquivo Excel de cotação que você preencheu.

Mapeamento de Fornecedores:

O sistema tentará sugerir colunas de preço do seu arquivo.

Mapeie as colunas de preço do Excel (ex: Volume 100 • $ Forn1) para os nomes dos fornecedores que você deseja (ex: Fornecedor Alpha).

Texto para 'Copiar Preço': O padrão é copiar. Pode alterá-lo se usar outra palavra.

Clique em "Importar Cotação Preenchida". O sistema importará os preços e os associará aos seus componentes e fornecedores.

5. Relatórios e Análises (Página "Relatórios e Análises")
Selecione a Moeda Base: Escolha a moeda na qual todos os custos e relatórios serão exibidos (ex: BRL, USD). As taxas de câmbio configuradas serão utilizadas.

Clique em "Gerar Relatórios".

Visualize os Relatórios:

Ranking de Preços: Tabela com os preços de cada componente por fornecedor e cenário, incluindo o ranking (melhor preço, 2º, etc.). Pode baixar para Excel.

Custo Total da BOM: O custo total otimizado da BOM para cada cenário de produção, baseado nos melhores preços.

Share de Custo por Fornecedor: Gráfico de pizza mostrando a contribuição percentual de cada fornecedor no custo total da BOM (usando os melhores preços).

Análise ABC: Tabela e gráfico de curva ABC que classificam os componentes pela sua contribuição acumulada no custo total.

Relatório de Diagnóstico: Clique em "Gerar Relatório de Diagnóstico" para ver um resumo dos logs de erro/aviso e métricas básicas do sistema.

🧪 Testes Unitários com Pytest
O projeto inclui uma pasta tests/ para testes unitários, utilizando a framework pytest. É crucial manter os testes atualizados para garantir a robustez das funcionalidades.

Como Executar os Testes
Ative o ambiente virtual do Poetry: poetry shell

Navegue até a raiz do projeto bom_costor/.

Execute os testes com pytest:

pytest

Escrevendo Testes
Crie arquivos de teste no diretório tests/ com o prefixo test_ (ex: test_importer.py).

Exemplo de Teste para importer.py
Você pode testar se a importação de uma BOM funciona corretamente, se o mapeamento de colunas está certo e se os dados são persistidos no DB.

# bom_costor/tests/test_importer.py

import pytest
import pandas as pd
from io import BytesIO
from sqlmodel import Session, select, delete
from bom_costor.core.importer import BomImporter
from bom_costor.db.models import BaseDB, Project, Bom, Component, BomComponent, RawComponent, Scenario, CurrencyRate

# Fixture para configurar e limpar o banco de dados para cada teste
@pytest.fixture(name="session_fixture")
def session_fixture():
    # Usa um banco de dados em memória para testes para isolamento
    BaseDB.DATABASE_URL = "sqlite:///:memory:"
    BaseDB.create_db_and_tables()
    with Session(BaseDB.engine) as session:
        yield session
        # Limpar o banco de dados após cada teste
        session.exec(delete(Price))
        session.exec(delete(ConsolidatedQuantity))
        session.exec(delete(BomComponent))
        session.exec(delete(RawComponent))
        session.exec(delete(Component))
        session.exec(delete(Bom))
        session.exec(delete(Project))
        session.exec(delete(Scenario))
        session.exec(delete(CurrencyRate))
        session.commit()

def test_import_bom_success(session_fixture: Session):
    importer = BomImporter()
    project_name = "Test Project Import"
    
    # Criar um DataFrame de teste
    data = {
        'Número da Peça': ['PN-001', 'PN-002'],
        'Descrição': ['Resistor', 'Capacitor'],
        'Fabricante': ['MFRX', 'MFRY'],
        'Qty por Montagem': [10, 5]
    }
    df = pd.DataFrame(data)
    
    # Simular upload de arquivo
    excel_buffer = BytesIO()
    df.to_excel(excel_buffer, index=False)
    excel_buffer.seek(0)

    # Mapeamento de colunas
    column_map = {
        'part_number': 'Número da Peça',
        'description': 'Descrição',
        'manufacturer': 'Fabricante',
        'qty_per_assy': 'Qty por Montagem'
    }
    scenarios_data = [{'name': 'Vol 100', 'volume': 100, 'multiplier': 1.0}]
    
    # Executar a importação
    imported_bom = importer.import_bom(
        excel_buffer,
        "test_bom.xlsx",
        project_name,
        column_map,
        scenarios_data
    )

    # Asserts
    assert imported_bom is not None
    assert imported_bom.file_name == "test_bom.xlsx"
    
    project = session_fixture.exec(select(Project).where(Project.name == project_name)).first()
    assert project is not None
    assert len(project.boms) == 1
    assert len(project.scenarios) == 1

    components = session_fixture.exec(select(Component)).all()
    assert len(components) == 2
    assert any(c.normalized_part_number == 'PN-001' for c in components)
    assert any(c.normalized_part_number == 'PN-002' for c in components)

    bom_components = session_fixture.exec(select(BomComponent)).all()
    assert len(bom_components) == 2
    assert all(bc.bom_id == imported_bom.id for bc in bom_components)
    
# Adicione mais testes para cenários de erro, validação de dados, etc.

Exemplo de Teste para normalizer.py
Testar a normalização de strings e a mesclagem de componentes duplicados.

# bom_costor/tests/test_normalizer.py

import pytest
from sqlmodel import Session, select, delete
from bom_costor.core.normalizer import ComponentNormalizer
from bom_costor.db.models import BaseDB, Component, BomComponent, Project, Bom, Scenario

@pytest.fixture(name="session_fixture")
def session_fixture():
    BaseDB.DATABASE_URL = "sqlite:///:memory:"
    BaseDB.create_db_and_tables()
    with Session(BaseDB.engine) as session:
        yield session
        session.exec(delete(BomComponent))
        session.exec(delete(Component))
        session.exec(delete(Bom))
        session.exec(delete(Project))
        session.exec(delete(Scenario))
        session.commit()

def test_normalize_string():
    normalizer = ComponentNormalizer()
    assert normalizer._normalize_string("PN-123.ABC") == "PN123ABC"
    assert normalizer._normalize_string("  PART_NUM  ") == "PARTNUM"
    assert normalizer._normalize_string("abc-123,xyz") == "ABC123XYZ"
    assert normalizer._normalize_string("") == ""
    assert normalizer._normalize_string(None) == "" # Testar com None

def test_find_and_merge_duplicates(session_fixture: Session):
    normalizer = ComponentNormalizer()

    # Crie um projeto e BOMs para que os componentes possam ter BomComponents
    project = Project(name="Project for Normalizer Test")
    session_fixture.add(project)
    session_fixture.commit()
    session_fixture.refresh(project)

    bom1 = Bom(project_id=project.id, file_name="bom1.xlsx")
    bom2 = Bom(project_id=project.id, file_name="bom2.xlsx")
    session_fixture.add(bom1)
    session_fixture.add(bom2)
    session_fixture.commit()
    session_fixture.refresh(bom1)
    session_fixture.refresh(bom2)


    # Adicione componentes com PNs similares
    comp1 = Component(normalized_part_number="PN-ALPHA")
    comp2 = Component(normalized_part_number="PNALPHA") # Duplicata fuzzy de comp1
    comp3 = Component(normalized_part_number="BETA-001")
    comp4 = Component(normalized_part_number="BETA001") # Duplicata fuzzy de comp3
    comp5 = Component(normalized_part_number="UNIQUE-ITEM") # Não deve ser mesclado

    session_fixture.add(comp1)
    session_fixture.add(comp2)
    session_fixture.add(comp3)
    session_fixture.add(comp4)
    session_fixture.add(comp5)
    session_fixture.commit()
    session_fixture.refresh(comp1)
    session_fixture.refresh(comp2)
    session_fixture.refresh(comp3)
    session_fixture.refresh(comp4)
    session_fixture.refresh(comp5)

    # Crie BomComponents que referenciam esses componentes
    bc1 = BomComponent(bom_id=bom1.id, component_id=comp1.id, qty_per_assy=1)
    bc2 = BomComponent(bom_id=bom2.id, component_id=comp2.id, qty_per_assy=2)
    bc3 = BomComponent(bom_id=bom1.id, component_id=comp3.id, qty_per_assy=3)
    bc4 = BomComponent(bom_id=bom2.id, component_id=comp4.id, qty_per_assy=4)
    bc5 = BomComponent(bom_id=bom1.id, component_id=comp5.id, qty_per_assy=5)
    session_fixture.add(bc1)
    session_fixture.add(bc2)
    session_fixture.add(bc3)
    session_fixture.add(bc4)
    session_fixture.add(bc5)
    session_fixture.commit()

    initial_component_count = session_fixture.exec(select(func.count(Component.id))).scalar_one()
    assert initial_component_count == 5

    # Execute a mesclagem
    merged_count = normalizer.find_and_merge_duplicates(threshold=85)

    assert merged_count == 2 # Esperamos 2 mesclagens (PN-ALPHA/PNALPHA e BETA-001/BETA001)

    final_component_count = session_fixture.exec(select(func.count(Component.id))).scalar_one()
    assert final_component_count == 3 # Três componentes únicos devem restar (PN-ALPHA, BETA-001, UNIQUE-ITEM)

    # Verifique se os BomComponents foram atualizados para apontar para o mestre
    updated_bc2 = session_fixture.get(BomComponent, bc2.id)
    assert updated_bc2.component_id == comp1.id # comp2 deve ter sido mesclado em comp1

    updated_bc4 = session_fixture.get(BomComponent, bc4.id)
    assert updated_bc4.component_id == comp3.id # comp4 deve ter sido mesclado em comp3

    # Verifique que o componente duplicado foi deletado
    deleted_comp2 = session_fixture.get(Component, comp2.id)
    assert deleted_comp2 is None
    deleted_comp4 = session_fixture.get(Component, comp4.id)
    assert deleted_comp4 is None

Esses exemplos cobrem a configuração do ambiente de teste, fixtures para gerenciar o banco de dados em memória (fundamental para testes isolados e rápidos) e exemplos básicos de como testar as funcionalidades de importação e normalização.

Para outros módulos (consolidator, pricing, reports), você seguiria um padrão semelhante:

Crie um arquivo test_*.py correspondente.

Use a session_fixture para garantir um DB limpo para cada teste.

Prepare os dados de entrada (criando objetos no DB ou DataFrames).

Chame o método da classe que você está testando.

Use assert para verificar se a saída está correta ou se o estado do DB foi alterado como esperado.

Espero que este README.md completo seja muito útil para si! Se tiver mais alguma dúvida ou precisar de ajustes, é só dizer.
