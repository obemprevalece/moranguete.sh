#!/bin/bash

# Cores (Tudo em Vermelho, exceto o Matrix que é Verde)
vermelho="\033[31m"
verde="\033[32m"
original="\033[0m"

clear

# Validação de Key
echo -e "${vermelho}======================================"
echo -e "       AUTENTICAÇÃO NECESSÁRIA"
echo -e "======================================${original}"
read -p "Digite a Key de acesso: " senha

if [ "$senha" != "1533" ]; then
    echo -e "${vermelho}Key inválida! Acesso negado.${original}"
    exit 1
fi

clear

# Função para exibir o menu limpo (Tudo em Vermelho)
mostrar_menu() {
    clear
    echo -e "${vermelho}======================================${original}"
    echo -e "${vermelho}           PAINEL DE CONTROLE         ${original}"
    echo -e "${vermelho}======================================${original}"
    echo -e "${vermelho} [1] Calibrar Mira & Otimizar Full    ${original}"
    echo -e "${vermelho} [2] Sair                             ${original}"
    echo -e "${vermelho}======================================${original}"
    echo -n "Input: "
}

# Escolha do Free Fire
escolher_ff() {
    clear
    echo -e "${vermelho}Escolha a versão do Free Fire:${original}"
    echo -e "${vermelho}[1] Free Fire Normal (com.dts.freefireth)${original}"
    echo -e "${vermelho}[2] Free Fire MAX (com.dts.freefiremax)${original}"
    read -p "Opção: " tipo_ff

    if [ "$tipo_ff" == "1" ]; then
        pacote="com.dts.freefireth"
    else
        pacote="com.dts.freefiremax"
    fi
}

# Efeito Matrix por 5 segundos (Mantido em VERDE conforme pedido)
efeito_matrix() {
    clear
    echo -e "${verde}"
    fim=$((SECONDS+5))
    while [ $SECONDS -lt $fim ]; do
        echo "01011001 11010011 00110101 11101000 10101101 01100101"
        echo "10010110 01101101 11010010 00101101 11011100 00101011"
        echo "11100101 00110111 10101011 01101101 00011101 11010010"
        sleep 0.2
    done
    echo -e "${original}"
}

executar_funcao1() {
    escolher_ff
    clear
    echo -e "${vermelho}[+] Calibrando mira e aplicando otimização FULL...${original}"
    
    # Otimizações seguras de toque e sensibilidade para evitar tremores (sem alterar DPI nem danificar o hardware)
    settings put system pointer_speed 0 2>/dev/null
    settings put system touch_exploration_enabled 0 2>/dev/null
    
    # Inicia o efeito Matrix de 5 segundos (Verde)
    efeito_matrix
    
    echo -e "${vermelho}Injetado, abrindo free fire...${original}"
    sleep 2
    
    # Comando para abrir o Free Fire automaticamente de forma segura
    am start -n $pacote/com.dts.freefireth.SplashActivity 2>/dev/null || am start --user 0 -a android.intent.action.MAIN -c android.intent.category.LAUNCHER -p $pacote 2>/dev/null
}

# Loop Principal
while true; do
    mostrar_menu
    read opcao
    case $opcao in
        1)
            executar_funcao1
            read -p "Pressione Enter para voltar ao menu..."
            ;;
        2)
            echo -e "${vermelho}Saindo...${original}"
            exit 0
            ;;
        *)
            echo -e "${vermelho}Opção inválida!${original}"
            sleep 1
            ;;
    esac
done
