/*
 * Programa de teste em User Space para validar a syscall get_proc_count()
 * Autor: Victor Ramos
 * Disciplina: Sistemas Operacionais - MINIX 3
 */

#include <stdio.h>
#include <stdlib.h>
#include <errno.h>

/* Protótipo da nova system call exposta pela libc */
int get_proc_count(void);

int main(void)
{
    int total_processos;

    printf("=== Teste da System Call get_proc_count() ===\n");
    printf("Invocando get_proc_count() via Process Manager (PM)...\n");

    total_processos = get_proc_count();

    if (total_processos < 0) {
        perror("Falha ao executar get_proc_count()");
        return EXIT_FAILURE;
    }

    printf("Numero de processos ativos no sistema: %d\n", total_processos);
    return EXIT_SUCCESS;
}