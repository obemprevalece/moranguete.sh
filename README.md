#!/system/bin/sh

# =========================================================
# SYSTEM & GAME OPTIMIZER - TERMUX
# =========================================================

clear
echo "=========================================="
echo "         SISTEMA DE AUTENTICAÇÃO          "
echo "=========================================="
echo ""
read -p "Digite a chave de acesso (Key): " KEY_INPUT

if [ "$KEY_INPUT" != "upredestined67" ]; then
    echo ""
    echo "[!] Chave incorreta! Acesso negado."
    exit 1
fi

echo ""
echo "[✓] Chave confirmada com sucesso!"
sleep 1

PKG_GAME=""
GAME_NAME=""

menu_jogo() {
    clear
    echo "=========================================="
    echo "              ESCOLHER JOGO               "
    echo "=========================================="
    echo ""
    echo "  [1] Free Fire Normal"
    echo "  [2] Free Fire Max"
    echo "  [3] Sair"
    echo ""
    read -p "Escolha o jogo: " OPCAO_JOGO

    case $OPCAO_JOGO in
        1)
            PKG_GAME="com.dts.freefireth"
            GAME_NAME="Free Fire Normal"
            ;;
        2)
            PKG_GAME="com.dts.freefiremax"
            GAME_NAME="Free Fire Max"
            ;;
        3)
            echo "Saindo..."
            exit 0
            ;;
        *)
            echo "Opção inválida!"
            sleep 1
            menu_jogo
            ;;
    esac
}

menu_jogo

# Função de Otimização Completa + Mira Pesada / Estável
executar_otimizacao_full() {
    clear
    echo "=========================================="
    echo "   OTIMIZAÇÃO FULL & MIRA - $GAME_NAME"
    echo "=========================================="
    echo ""
    echo "🔄 Adjustando estabilidade e precisão do toque (Mira Pesada)..."
    
    # Reduz a aceleração brusca do ponteiro para deixar a mira firme/pesada
    settings put system pointer_speed 3 > /dev/null 2>&1

    # Otimiza o tempo de resposta do toque (Touch Sampling)
    settings put secure long_press_timeout 250 > /dev/null 2>&1
    settings put secure multi_press_timeout 200 > /dev/null 2>&1

    echo "🔄 Otimizando transições de GPU e sistema..."
    
    # Reduz atraso visual de quadros
    settings put global window_animation_scale 0.5 > /dev/null 2>&1
    settings put global transition_animation_scale 0.5 > /dev/null 2>&1
    settings put global animator_duration_scale 0.5 > /dev/null 2>&1
    settings put global force_gpu_rendering 1 > /dev/null 2>&1

    echo "🔄 Executando limpeza completa do sistema e cache..."
    
    # Limpeza profunda de caches
    pm trim-caches 999G > /dev/null 2>&1

    if [ -n "$PKG_GAME" ]; then
        pm trim-caches 999G $PKG_GAME > /dev/null 2>&1
    fi

    echo ""
    echo "=========================================="
    echo "[✓] PROCESSO CONCLUÍDO COM SUCESSO!"
    echo "=========================================="
    echo ""
    read -p "Pressione ENTER para voltar ao menu..."
}

# Desenho ASCII Matrix (Ghostbusters)
desenhar_logo() {
cat << 'EOF'
. .  .  . .  .  . .  .  . . . .:%8@8@;. . .  .  . .  .  . .  .  . .  .  .  
   .       .       .       .   ..:@.; S8XS;X%%; .   .      .       .       .   .
     .  .    .  .    .  .    .;8 tS@. ...S8S8@S%       . .    . .    .  .    .  
 .       .       .       . .;%S S@t....::.8.:t:X .. .       .     .      .      
   .  .    .  .    .  .   .%S.;S%...t8 ::X:88SS t8@t:.....     .    . .    .  . 
  .    .  .    .  .    . ..:S 8X :  ::;..t       .8X.8X:   . .   .      .   .   
    .       .       .  ..;8@X..S:..8@%....  8  8 .   ;@ 8@: .      .  .   .    .
  .   . .    .  .  . .:@.888...tX  :%...  .  8  8 ::8   .8 SS. ..       .    .  
    .     .    .  . ;88 88::. . .... . . @88  8 .8 :.8 8     .@: . . .     .    
  .    .   .    . .t; .t% : ..  ::@.  . : t88 ;8  8 ..8 .8 . :X@; .    .  .   . 
     .   .   . ..:@  t; :.     .t.S..   .. @888888.8.8 :8 8.8 t ;X..    .   .   
  .    .      ..;%::8@X:.  .  . X8. .    . 8888888888;;8 ::8 ;  8.8; .         .
    .     . .  ;@;8..88  .   . .t;8..   .. X8 :;X. 8%8@ ;8  8.8  :@8...  . .  . 
  .   .      ..88 .8  8S.. .  .. @8 . .  ..X8  ..  tS%8t. 8 .   8.8 8...     .  
    .   . ....88 ..:8 ;X ;.       ;.    .;.8..  .t:8.:;8 8 :8 8 .  8t8...  .    
  .        . % .;8 :88:@8:. . .:.X%.. . .X:..:.: .  :8..8 8   8 8 :. X::8t.   . 
     . . . .:8;t8 8.%%::.:.    .;X..  .  ...:SX .;8:8.8.:::8 8 :.8t.X;X.%X; .   
;X.  %8%:.. % .8.:888%.     .    .    ...t;@.  8:;:8 ::8 8.@8.%88 8:.8%X:8.    .
@@;X@8X .;8%88.::8:.;. .  .    .  ....%.8   ;8.:8:8 ;8 ;% :  .tt .:%8t%:;    .  
;8 t@;  .%88S:88 %:: .      .    .; ;    ;8 8 8 .8 8 t:S..... ..   888:SXt..    
.X;SX8S....8S%;;;..   . ..   ... ;%X  ;;8 .8.::8 :;X %S8;...     ..8X@8@S.t8. . 
SS%SSX@.  .. .. .  . . . tt. .t:8   :8.8 8..8:;: %;:.8@8.. . .... .; 8.S8S% S   
tX:t%8.X..       . .. %%;8. ;    8.8 .:.::8 :8XX. . S8%: .%:8.S..@ :8%;X%;;X:  .
 ;X 8@8t8:. .... ...%8SX8@S8  .8.::.8 8.8 ;X 8;.:. .8StSt:.t8@8%;@8:%8  XSt..   
  :X;;; :X;: @...888% 8XXt  8 ::8.8 :::;X.%.;:  .   .:@S ...@ ;8:   X8S%8:      
  ..%Xt.;8t..8  SS.8.:8:.8.. 8 8 .:8:8@X  :. .   .   ..  ..8 .8 8.8 : %8;  .  . 
   .    ...:XX.8X8 .8 :8 ;8.8   8.8;;8;... .   .   .   ...%8.;:::8 :@ ;:..      
 .   .    .. S    8 .8.:8..8  8 8S88@;.  .  .    .   . ..@8 .8.8 .8.  S     .   
   .   .   . .S..8 :8 ;8 8..8;@@8@ 8:   S88@. .   . ...:   :8 ..8 ;; 8 . .    . 
     .   .  . :@  .8 ;8 .:8 88@t8@ :.:. t@8% .  . . : %8  8..8:: 8X;S:.    .    
 .     .     . tX .:8..8 ::;S8S:t..  ...  . .  .. ..@   :8 8..8.;S8t; . .    .  
   . .    .    .;8t;:8  8 8 8 t8888:. . .. . .. S8S    8 ::.8  @ S8:  .   .     
  .     .    .  ..S8@    8 :.8     8S:8X;;;%88S8 . .8 :.8.8   8 @8:.    .   .  .
     .    .     .. @%  8  8.8 ;8 .        .     :;8 .8.:...:8%X X..  .          
  .    .     .    . ;S8t8.:8 t8 :8  8  8 8 8 :8.8 .;8 8 8 % @ @:.     .  .  .   
    .    . .   .   ...:8X%8 :8.8  8  8  ..8 ;8 ..8  :: ;@ @ 8;.   . .     .   . 
  .   .          .    ..t8S: .8  ..8  8 8  8 .8   8.@@   .8:...       .         
    .   .  . .          .:.;%SX%: @88.8  8 :8 8@XS   .88; ...   . .    .  . .   
  .            .  . .  .   .  ..%X@8@8%8@%%@:@%%888S:.:.     .      .         . 
EOF
}

while true; do
    clear
    echo "=========================================="
    echo "     PAINEL DE FUNÇÕES - $GAME_NAME      "
    echo "=========================================="
    echo ""
    echo "  [1] Otimização Full"
    echo "  [2] Sair"
    echo ""
    desenhar_logo
    echo ""
    read -p "Escolha uma opção: " OPCAO_MENU

    case $OPCAO_MENU in
        1)
            executar_otimizacao_full
            ;;
        2)
            echo "Saindo do script..."
            exit 0
            ;;
        *)
            echo "Opção inválida!"
            sleep 1
            ;;
    esac
done
