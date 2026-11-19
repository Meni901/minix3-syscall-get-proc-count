# System Call `get_proc_count()` no MINIX 3 — Registro Parcial

**Disciplina:** Sistemas Operacionais  
**Aluno:** Victor Ramos  
**System Call:** `get_proc_count()` — Retorna o número de processos ativos no sistema.

---

## 1. Situação da Entrega

**Não consegui concluir a implementação prática da system call `get_proc_count()` no MINIX 3.**

Este repositório reúne a estrutura de arquivos e os códigos de referência(`getproccount.c`, `get_proc_count.c` e `test_proc_count.c`), além do relatório técnico documentando a arquitetura teórica e os problemas que impediram a execução e a compilação efetiva do sistema.

---

## 2. Por Que Não Consegui Implementar

1. **Ausência do código-fonte (`/usr/src`):** Após instalar o MINIX 3 na VM (`MV2`), o diretório `/usr/src` não existia no sistema, o que também impediu rodar `make build` para validar a compilação base.
2. **Falhas no download via rede (`git` e `pkgin`):** As tentativas de baixar os fontes diretamente na VM não funcionaram.
3. **Falha ao montar a ISO dentro da VM:** Ao tentar montar o disco óptico (`/dev/cd0a`) em `/mnt/iso` com `cd9660`, ocorreu erro de argumento inválido (`Invalid argument`), e o log do VirtualBox indicou a unidade óptica vazia (`EmptyDrive = 1`) na VM `MV2`.
4. **Limitação de ambiente e execução:** Sem os fontes completos e com as dificuldades de ambiente na VM, não foi possível alterar os arquivos reais do sistema, recompilar o kernel/PM nem testar o programa em C compilado contra a nova syscall.

---

## 3. Estrutura do Repositório

```text
minix3-syscall-get-proc-count/
├── README.md
├── docs/
│   └── RELATORIO_TECNICO.md
├── src/
│   ├── servers/
│   │   └── pm/
│   │       └── getproccount.c
│   └── lib/
│       └── libc/
│           └── misc/
│               └── get_proc_count.c
└── user/
    └── test_proc_count.c
```

* `docs/RELATORIO_TECNICO.md`: Relato detalhado da minha situação, por que não consegui implementar e como funcionaria a chamada na arquitetura do MINIX 3.
* `src/servers/pm/getproccount.c`: Rascunho de referência para `do_get_proc_count()` no Process Manager.
* `src/lib/libc/misc/get_proc_count.c`: Rascunho de referência do wrapper na `libc`.
* `user/test_proc_count.c`: Programa de teste em C de referência para user space.