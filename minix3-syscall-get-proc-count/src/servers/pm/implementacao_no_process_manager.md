/*
 * Implementação da System Call get_proc_count() no Process Manager (PM)
 * Autor: Victor Ramos
 * Disciplina: Sistemas Operacionais - MINIX 3
 */

#include "pm.h"
#include <minix/callnr.h>
#include <minix/com.h>
#include "mproc.h"

/*===========================================================================*
 *				do_get_proc_count			     *
 *===========================================================================*/
int do_get_proc_count(void)
{
    register struct mproc *rmp;
    int active_procs = 0;
    int i;

    /* Percorre toda a tabela de processos gerenciada pelo Process Manager */
    for (i = 0; i < NR_PROCS; i++) {
        rmp = &mproc[i];

        /* Contabiliza apenas os slots que possuem um processo ativo */
        if (rmp->mp_flags & IN_USE) {
            active_procs++;
        }
    }

    /* Armazena a contagem na mensagem de resposta IPC para o chamador */
    mp->mp_reply.m_lc_pm_getproccount.count = active_procs;

    return(OK);
}