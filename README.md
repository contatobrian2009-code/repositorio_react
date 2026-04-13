# Android Task App — Navegação com Intents

Projeto de estudo que implementa navegação entre duas Activities usando Intents explícitos.

## O que o app faz

Tela principal (`MainActivity`) exibe uma lista de tarefas num `RecyclerView`. Um `FloatingActionButton` abre a segunda tela (`AddTaskActivity`), onde o usuário digita a descrição e salva. A tarefa volta para a lista automaticamente.

## Fluxo de navegação

```
MainActivity
    └── FAB clicado
            └── startActivityForResult(intent, REQUEST_ADD_TASK)
                        └── AddTaskActivity
                                └── setResult(RESULT_OK, intent)
                                        └── finish()
                                                └── onActivityResult() na MainActivity
                                                        └── adapter.notifyItemInserted(0)
```

## Estrutura do projeto

```
app/src/main/java/
├── MainActivity.java       # tela principal, RecyclerView, onActivityResult
├── AddTaskActivity.java    # formulário de nova tarefa, setResult + finish
└── TaskAdapter.java        # adapter do RecyclerView

app/src/main/res/layout/
├── activity_main.xml       # RecyclerView + FloatingActionButton
├── activity_add_task.xml   # EditText + Button Salvar
└── item_task.xml           # layout de cada item da lista
```

## Trechos principais

**Abrindo a segunda tela (MainActivity):**
```java
Intent intent = new Intent(MainActivity.this, AddTaskActivity.class);
startActivityForResult(intent, REQUEST_ADD_TASK);
```

**Retornando o resultado (AddTaskActivity):**
```java
Intent result = new Intent();
result.putExtra("task_description", desc);
setResult(RESULT_OK, result);
finish();
```

**Recebendo na MainActivity:**
```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if (requestCode == REQUEST_ADD_TASK && resultCode == RESULT_OK && data != null) {
        String novaDescricao = data.getStringExtra("task_description");
        taskList.add(0, novaDescricao);
        adapter.notifyItemInserted(0);
    }
}
```

## Observação

`startActivityForResult` está deprecated a partir do API 30. Em projetos novos o recomendado é usar `ActivityResultLauncher` com `registerForActivityResult()`, mas a lógica de Intent + resultado é a mesma.

## Tecnologias

- Java
- Android SDK
- RecyclerView
- FloatingActionButton (Material Components)
