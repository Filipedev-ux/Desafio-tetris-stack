# Desafio-tetris-stack
Desafio tetris Stack
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

#define TAMANHO_FILA 5

typedef struct {
    char nome;
    int id;
} Peca;

typedef struct {
    Peca itens[TAMANHO_FILA];
    int inicio;
    int fim;
    int quantidade;
} FilaCircular;

int proximoId = 1;

Peca gerarPeca() {
    Peca novaPeca;
    char tipos[] = {'I', 'O', 'T', 'L', 'J', 'S', 'Z'};
    novaPeca.nome = tipos[rand() % 7];
    novaPeca.id = proximoId++;
    return novaPeca;
}

void inicializarFila(FilaCircular *fila) {
    fila->inicio = 0;
    fila->fim = -1;
    fila->quantidade = 0;
    for (int i = 0; i < TAMANHO_FILA; i++) {
        Peca nova = gerarPeca();
        fila->fim = (fila->fim + 1) % TAMANHO_FILA;
        fila->itens[fila->fim] = nova;
        fila->quantidade++;
    }
}

void visualizarFila(FilaCircular *fila) {
    if (fila->quantidade == 0) {
        printf("\nFila de pecas futuras esta vazia.\n\n");
        return;
    }

    printf("\n--> Fila de Pecas Futuras <--\n\n");
    int i = fila->inicio;
    int count;
    for (count = 0; count < fila->quantidade; count++) {
        printf("  %d: Peca[ID: %d, Tipo: %c]\n", count + 1, fila->itens[i].id, fila->itens[i].nome);
        i = (i + 1) % TAMANHO_FILA;
    }
    printf("\n--------------------------------\n");
}

Peca desenfileirar(FilaCircular *fila) {
    Peca pecaRemovida;
    if (fila->quantidade == 0) {
        pecaRemovida.id = -1;
        pecaRemovida.nome = ' ';
        return pecaRemovida;
    }

    pecaRemovida = fila->itens[fila->inicio];
    fila->inicio = (fila->inicio + 1) % TAMANHO_FILA;
    fila->quantidade--;
    return pecaRemovida;
}

void enfileirar(FilaCircular *fila, Peca novaPeca) {
    if (fila->quantidade == TAMANHO_FILA) {
        return;
    }
    fila->fim = (fila->fim + 1) % TAMANHO_FILA;
    fila->itens[fila->fim] = novaPeca;
    fila->quantidade++;
}

int main() {
    srand(time(NULL));
    FilaCircular filaDePecas;
    inicializarFila(&filaDePecas);
    int opcao;

    do {
        printf("\n*** Tetris Stack - Nivel Novato ***\n");
        printf("1. Visualizar Fila de Pecas\n");
        printf("2. Jogar Peca\n");
        printf("0. Sair\n");
        printf("Escolha uma opcao: ");
        scanf("%d", &opcao);

        switch (opcao) {
            case 1:
                visualizarFila(&filaDePecas);
                break;
            case 2:
                if (filaDePecas.quantidade > 0) {
                    Peca jogada = desenfileirar(&filaDePecas);
                    printf("\n>> Peca [ID: %d, Tipo: %c] foi jogada! <<\n", jogada.id, jogada.nome);

                    Peca nova = gerarPeca();
                    enfileirar(&filaDePecas, nova);
                    printf(">> Nova peca [ID: %d, Tipo: %c] inserida na fila! <<\n", nova.id, nova.nome);

                    visualizarFila(&filaDePecas);
                } else {
                    printf("\nA fila esta vazia, nao ha pecas para jogar.\n");
                }
                break;
            case 0:
                printf("\nObrigado por jogar Tetris Stack! Saindo...\n");
                break;
            default:
                printf("\nOpcao invalida! Tente novamente.\n");
                break;
        }
    } while (opcao != 0);

    return 0;
}
