/*
 * Wrapper de biblioteca (libc) para a syscall get_proc_count()
 * Autor: Victor Ramos
 * Disciplina: Sistemas Operacionais - MINIX 3
 */

#include <lib.h>
#include <unistd.h>
#include <string.h>

int get_proc_count(void)
{
    message m;
    int status;

    memset(&m, 0, sizeof(m));

    /* Envia mensagem IPC síncrona ao Process Manager (PM_PROC_NR) */
    status = _syscall(PM_PROC_NR, PM_GETPROCCOUNT, &m);
    if (status < 0) {
        return(-1);
    }

    return(m.m_lc_pm_getproccount.count);
}