### 4차시 실습

- 이번 실습에서는 간단한 벡터 데이터베이스 실습을 진행합니다.
- 벡터 데이터베이스 기초 설명은 [VectorDB_RAG.pdf](./VectorDB_RAG.pdf)에 있습니다.
  <br/>
- 실습에서는 벡터 데이터베이스 중 ChromaDB를 사용합니다. 2024년 7월 26일 기준 구글 코랩에서 돌아가는 ipynb 파일을 제공합니다. (파이썬 버전 3.10.12 (main, Mar 22 2024, 16:50:05) [GCC 11.4.0])
- ChromaDB Docs: https://docs.trychroma.com/getting-started
- ChromaDB 실습 자료는 [chromadb_getting_started.ipynb](./chromadb_getting_started.ipynb), [chromadb_RAG.ipynb](./chromadb_RAG.ipynb) 입니다.

<br/>

- 위 예시처럼 document에 string을 직접 넣는 것이 아닌, csv file 내 블로그 글을 직접 파싱하고 넣어보는 예시는 RAG_example 폴더에 있습니다. ([실습 파일](./RAG_example/exercise.ipynb))
- 다만, 해당 예시는 postgres extension인 pgvector에 vector를 저장하니 참고 바랍니다.

<br/>

- Python에서 PostgreSQL을 사용하는 방법은 python_pg 폴더에 있습니다. 지금까지 실습한 것처럼 PostgreSQL 데이터베이스 내부에서 직접 데이터를 조작하는 대신, Python 코드에서 PostgreSQL 서버에 연결하여 기본 SQL 명령어를 실행할 수 있도록 설정하였습니다. CREATE, INSERT, DELETE, SELECT 등의 기초 SQL 코드가 포함되어 있습니다. 향후 프로젝트에서 참고하시면 유용할 것입니다.
