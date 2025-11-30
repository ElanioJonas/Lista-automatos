# Lista-automatos
# ------------------------------
# Simulador de AFD (Autômato Finito Determinístico)
# O usuário declara o autômato e depois testa cadeias.
# ------------------------------

class DFA:
    def __init__(self, estados, alfabeto, transicoes, estado_inicial, estados_finais):
        """
        estados: conjunto (ou lista) com os nomes dos estados (strings)
        alfabeto: conjunto (ou lista) com os símbolos do alfabeto (strings)
        transicoes: dicionário {(estado, simbolo): proximo_estado}
        estado_inicial: nome do estado inicial (string)
        estados_finais: conjunto (ou lista) com os estados de aceitação
        """
        self.estados = set(estados)
        self.alfabeto = set(alfabeto)
        self.transicoes = transicoes
        self.estado_inicial = estado_inicial
        self.estados_finais = set(estados_finais)

    def aceita(self, cadeia):
        """Simula a execução do AFD sobre a cadeia."""
        estado_atual = self.estado_inicial

        for simbolo in cadeia:
            if simbolo not in self.alfabeto:
                print(f"Símbolo '{simbolo}' não pertence ao alfabeto. Cadeia rejeitada.")
                return False

            chave = (estado_atual, simbolo)
            if chave not in self.transicoes:
                # Não há transição definida (AFD trava)
                print(f"Não há transição a partir de ({estado_atual}, '{simbolo}'). Cadeia rejeitada.")
                return False

            estado_atual = self.transicoes[chave]

        # Ao final da leitura, verifica se está em estado final
        return estado_atual in self.estados_finais


def ler_afd_do_usuario():
    print("==== DEFINIÇÃO DO AFD ====")

    # Estados
    estados = input("Informe os estados separados por espaço (ex: q0 q1 q2): ").split()

    # Alfabeto
    alfabeto = input("Informe o alfabeto separado por espaço (ex: 0 1): ").split()

    # Estado inicial
    estado_inicial = input("Informe o estado inicial: ").strip()
    while estado_inicial not in estados:
        print("Estado inicial inválido. Digite um estado que esteja na lista de estados.")
        estado_inicial = input("Informe o estado inicial: ").strip()

    # Estados finais
    estados_finais = input("Informe os estados de aceitação separados por espaço: ").split()
    for e in estados_finais:
        if e not in estados:
            print(f"Aviso: estado final '{e}' não está na lista de estados!")

    # Transições
    print("\nAgora vamos definir as transições.")
    print("Formato: estado_atual símbolo próximo_estado")
    print("Digite uma linha vazia para terminar.\n")

    transicoes = {}

    while True:
        linha = input("Transição (ex: q0 0 q1): ").strip()
        if linha == "":
            break

        partes = linha.split()
        if len(partes) != 3:
            print("Entrada inválida. Use o formato: estado_atual símbolo próximo_estado\n")
            continue

        e_atual, simbolo, e_prox = partes

        if e_atual not in estados:
            print(f"Estado '{e_atual}' não está na lista de estados.")
            continue
        if simbolo not in alfabeto:
            print(f"Símbolo '{simbolo}' não pertence ao alfabeto.")
            continue
        if e_prox not in estados:
            print(f"Próximo estado '{e_prox}' não está na lista de estados.")
            continue

        chave = (e_atual, simbolo)
        if chave in transicoes:
            print(f"Atenção: transição de ({e_atual}, '{simbolo}') já definida. Substituindo.")
        transicoes[chave] = e_prox

    print("\nAFD definido com sucesso!\n")
    return DFA(estados, alfabeto, transicoes, estado_inicial, estados_finais)


def main():
    afd = ler_afd_do_usuario()

    print("==== TESTE DE CADEIAS ====")
    print("Digite uma cadeia para testar (ENTER vazio para sair).")

    while True:
        cadeia = input("Cadeia: ")
        if cadeia == "":
            print("Encerrando.")
            break

        if afd.aceita(cadeia):
            print("Resultado: A CE I T A\n")
        else:
            print("Resultado: R E J E I T A D A\n")


if __name__ == "__main__":
    main()
