# Sistema-de-Gest-o-de-Busca-pesquisa-em-Bibliotecas-P-blica-Munic-pio-de-Reden-o-Ce
API mobile da Biblioteca pública do Municipio de Redenção. Este projeto visa modernizar o acesso ao acervo da Biblioteca Pública Municipal de Redenção, Ceará, permitindo consulta, verificação de disponibilidade e reserva de livros de forma digital.
# 📚 Sistema de Gestão de Acervo - Biblioteca Pública de Redenção - Ceará

Este projeto visa modernizar o acesso ao acervo da Biblioteca Pública Municipal de Redenção, Ceará, permitindo consulta, verificação de disponibilidade e reserva de livros de forma digital.

## 🚀 Tecnologias Utilizadas

| Componente | Tecnologia | Descrição |
| :--- | :--- | :--- |
| **Backend (API)** | **Python / FastAPI** | API robusta e de alta performance. |
| **Frontend (Mobile)** | **[Inserir Tecnologia - Ex: Flutter/React Native]** | Aplicativo móvel para acesso dos cidadãos. |
| **Banco de Dados** | **[Inserir DB - Ex: PostgreSQL/SQLite]** | Armazenamento do catálogo e dados de usuários. |

## 📦 Estrutura do Projeto

O projeto é dividido em `backend/` (FastAPI) e `mobile-app/`. Consulte a raiz de cada pasta para detalhes de setup.

## 🛠️ Como Iniciar o Backend (API)

**Pré-requisitos:** Python 3.9+ e Pip.

1. **Clone o repositório:**
   ```bash
   git clone [URL_DO_REPO]
   cd biblioteca-redencao/backend
Endpoints Principais
Rota	Método	Descrição
/livros/	GET	Lista todos os livros ou realiza a busca.
/livros/{id}	GET	Detalhes de um livro específico.
/reservas/	POST	Cria uma nova reserva.
/docs	GET	Documentação interativa da API (Swagger UI).
# Modelos Pydantic para tipagem e validação
from pydantic import BaseModel, Field
from typing import Optional

class LivroBase(BaseModel):
    titulo: str = Field(..., example="O Alquimista")
    autor: str = Field(..., example="Paulo Coelho")
    ano_publicacao: int = Field(..., example=1988)
    # isbn: str

class Livro(LivroBase):
    id: int = Field(..., example=1)
    disponivel: bool = Field(..., example=True)

    class Config:
        orm_mode = True # Necessário se usar SQLAlchemy
        from fastapi import APIRouter, Depends, HTTPException, Query
from typing import List, Optional
from database.models import Livro # Importa o modelo Pydantic

# Instância do roteador
router = APIRouter(
    prefix="/livros",
    tags=["Livros"] # Agrupa na documentação Swagger
)

# Simulando um banco de dados
DADOS_LIVROS = [
    {"id": 1, "titulo": "A Culpa é das Estrelas", "autor": "John Green", "ano_publicacao": 2012, "disponivel": True},
    {"id": 2, "titulo": "Dom Casmurro", "autor": "Machado de Assis", "ano_publicacao": 1899, "disponivel": False},
]

# --- Rota Principal: Listagem e Busca ---
@router.get("/", response_model=List[Livro])
def listar_e_buscar_livros(
    busca: Optional[str] = Query(None, description="Termo de busca (título ou autor)"),
    disponivel: Optional[bool] = Query(None, description="Filtra por disponibilidade")
):
    """
    Lista todos os livros no acervo.
    Permite busca por título/autor e filtro por disponibilidade.
    """
    resultados = DADOS_LIVROS

    if busca:
        busca = busca.lower()
        resultados = [
            l for l in resultados
            if busca in l["titulo"].lower() or busca in l["autor"].lower()
        ]

    if disponivel is not None:
        resultados = [
            l for l in resultados
            if l["disponivel"] == disponivel
        ]

    return resultados

# --- Rota de Detalhe ---
@router.get("/{livro_id}", response_model=Livro)
def obter_detalhes_livro(livro_id: int):
    """
    Retorna os detalhes de um livro específico pelo ID.
    """
    for livro in DADOS_LIVROS:
        if livro["id"] == livro_id:
            return livro
    
    raise HTTPException(status_code=404, detail="Livro não encontrado")
    from fastapi import FastAPI
from routers import livros
# from routers import reservas # Para quando for implementar
# from database.connection import init_db # Se usar DB real

app = FastAPI(
    title="API Biblioteca Redenção",
    description="API de consulta e reserva de livros para a BPM de Redenção.",
    version="1.0.0"
)

# Inclui as rotas do módulo livros
app.include_router(livros.router)

# @app.on_event("startup")
# async def startup_event():
#     init_db()
