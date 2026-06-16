#include <iostream>
#include <cstdlib>
#include <ctime>

//Prototipo de funcao
void exibirJogoDaMemoria(std::string jogoDaMemoriaUsuario[4][4], int placar);

int main()
{
    
    //Menu para repetir todo o jogo
	int menu=1;
	while(menu!=0) {
	    
	    //Declaracao de variaveis utilizadas no codigo
		int letras=0, line_rand=0, coln_rand=0, pontuacao=0, confirmacao_saida=0;
		int jogada_x=0, jogada_y=0, copia_x=0, copia_y=0, tentativas=0, placar=0, copia_placar=0;
		std::string p_continuar;
		
        //Declaracao das matrizes e vetor
		std::string jogoDaMemoriaUsuario[4][4];
		std::string jogoDaMemoria[4][4];
		std::string letrasMemoria[8] = {"\tA","\tB","\tC","\tD","\tE","\tF","\tG","\tH"};

        //For para preencher o jogo da memoria e o
        //jogo da memoria do usuario com casas vazias
		for(int y=0; y<4; y++) {
			for(int x=0; x<4; x++) {
				jogoDaMemoria[y][x]="\t-";
				jogoDaMemoriaUsuario[y][x]="\t-";
			}
		}
		
        //Funcao para chamar o jogo da memoria e exibir
		exibirJogoDaMemoria(jogoDaMemoriaUsuario, placar);

        //Gerador de numeros aleatorios
		srand(time(0));

        //For para preencher os lugares do jogo da memoria
        //com as letras em pares
		for(int l=0; l<8; l++) {

            //While para repetir ate colocar duas letras em
            //duas posicoes diferentes
			letras=0;
			while(letras<2) {

                //Posicoes aleatorias
				line_rand = rand() % 4;
				coln_rand = rand() % 4;

                //Condicoes para colocar em casas vazias
                //Sem repetir letras e colocando 2 pares
                //ate preencher todo o jogo da memoria
				if(jogoDaMemoria[line_rand][coln_rand]=="\t-") {
					if(letras<=2) {
						jogoDaMemoria[line_rand][coln_rand] = letrasMemoria[l];
						letras++;
					}
				}
			}
		}

        //While para rodar todo o jogo
		int jogo_rodando=1;
		while(jogo_rodando!=0) {

            //Limpeza do console e exibir o jogo
			system("clear");
			exibirJogoDaMemoria(jogoDaMemoriaUsuario, placar);


            //While para tornar a jogada valida
			int menu_primeira_jogada_valida=1;
			while(menu_primeira_jogada_valida!=0) {
			    
			    //Primeira jogada
				std::cout << "Insira sua jogada [X]: ";
				std::cin >> jogada_x;
				std::cout << "Insira sua jogada [Y]: ";
				std::cin >> jogada_y;
				
                //If com a jogada e pegando uma copia da primeira jogada
				if(jogada_x >= 0 && jogada_x <= 4 && jogada_y >=0 && jogada_y <=4) {
					if(jogoDaMemoriaUsuario[jogada_y][jogada_x]=="\t-") {
						copia_x = jogada_x;
						copia_y = jogada_y;
						menu_primeira_jogada_valida = 0;
						break;
					}
				}

				system("clear");
				std::cout << "Insira uma jogada valida!" << std::endl << std::endl;
				exibirJogoDaMemoria(jogoDaMemoriaUsuario, placar);

			}
			
            //Limpeza do sistema e colocando para o jogo da memoria do usuario
            //exibir a letra que o jogador acertou
			system("clear");
			jogoDaMemoriaUsuario[jogada_y][jogada_x] = jogoDaMemoria[jogada_y][jogada_x];
			exibirJogoDaMemoria(jogoDaMemoriaUsuario, placar);

            //While para a segunda jogada valida
			int menu_segunda_jogada_valida=1;
			while(menu_segunda_jogada_valida!=0) {

                //Segunda jogada
				std::cout << "Insira sua segunda jogada [X]: ";
				std::cin >> jogada_x;
				std::cout << "Insira sua segunda jogada [Y]: ";
				std::cin >> jogada_y;

				if(jogada_x >= 0 && jogada_x <= 4 && jogada_y >=0 && jogada_y <=4) {
					if(jogoDaMemoriaUsuario[jogada_y][jogada_x]=="\t-") {
						menu_segunda_jogada_valida = 0;
						break;
					}
				}

				system("clear");
				std::cout << "Insira uma jogada valida!" << std::endl << std::endl;
				exibirJogoDaMemoria(jogoDaMemoriaUsuario, placar);
			}
			
			
    
            //If para comparar as casas, com a copia da primeira e a segunda jogada
            //Se forem iguais, soma a pontuacao para finalizar o jogo
            //Faz o calculo de pontos no placar e exibe a mensagem
            //de acerto e zera as tentativas
			if(jogoDaMemoria[jogada_y][jogada_x]==jogoDaMemoria[copia_y][copia_x]) {
				pontuacao++;

				placar=(1*55-tentativas)+placar;
				
				jogoDaMemoriaUsuario[jogada_y][jogada_x] = jogoDaMemoria[jogada_y][jogada_x];
				std::cout << "Voce acertou um par!";
				exibirJogoDaMemoria(jogoDaMemoriaUsuario, placar);
				tentativas=1;
				
			} else {

				system("clear");

				jogoDaMemoriaUsuario[jogada_y][jogada_x] = jogoDaMemoria[jogada_y][jogada_x];
				tentativas++;
				exibirJogoDaMemoria(jogoDaMemoriaUsuario, placar);
				std::cout << "Voce errou, tente novamente!" << std::endl;

				std::cout << "Digite qualquer tecla pra continuar." << std::endl;
				std::cin >> p_continuar;

				jogoDaMemoriaUsuario[jogada_y][jogada_x] = "\t-";
				jogoDaMemoriaUsuario[copia_y][copia_x] = "\t-";
				exibirJogoDaMemoria(jogoDaMemoriaUsuario, placar);
			}
			//Se for diferentes, a copia da primeira e a da segunda jogada
			//Limpa o sistema, mostra que voce errou
			//soma as tentativas e limpa o tabuleiro do usuario
			//Nos lugares que o jogador colocou

            //if para finalizar o jogo quando a pontuacao for 8
            //Ou seja, ter acertado os 8 pares
			if(pontuacao==8) {
				jogo_rodando=0;
				system("clear");
				std::cout << "\nParabens! Voce venceu!\n";
				break;
			}
		}

        //While para repetir um menu de rejogar ou nao
		int rejogar=1;
		while(rejogar!=0) {

			exibirJogoDaMemoria(jogoDaMemoriaUsuario, placar);
			std::cout << "\nDeseja jogar novamente?\n";
			std::cout << "0 - Nao.\n";
			std::cout << "1 - Sim.\n";
			std::cout << "Digite aqui: ";
			std::cin >> confirmacao_saida;

			int validar_rejogar=1;
			while(validar_rejogar!=0) {

				if(confirmacao_saida==0) {
					return 0;
				}
				else if(confirmacao_saida==1) {
					rejogar=0;
					break;
				}
				system("clear");
				exibirJogoDaMemoria(jogoDaMemoriaUsuario, placar);
				std::cout << "Insira uma opcao valida!\n";
			}
		}
	}
}

//Funcao para exibir o tabuleiro
void exibirJogoDaMemoria(std::string jogoDaMemoriaUsuario[4][4], int placar)
{
    
    //Menu bonito com titulo e pontuacao
	std::cout << "////////////////////////////////////////////////" << std::endl;
	std::cout << "//              JOGO DA MEMORIA               //" << std::endl;
	std::cout << "////////////////////////////////////////////////" << std::endl;
	std::cout << "Sua pontuacao e de " << placar << " pontos!" << std::endl << std::endl;

	//Linha de cima do jogo
	for(int i=0; i<4; i++) {
		std::cout << "\t" << i;
	}
	std::cout << "   " << std::endl;
	for(int i=0; i<4*9; i++) {
		std::cout << "_";
	}
	std::cout << "" << std::endl;
	std::cout << "" << std::endl;

	//Exibicao do jogo
	for(int y=0; y<4; y++) {
		for(int x=0; x<4; x++) {
			std::cout << jogoDaMemoriaUsuario[y][x];
		}
		std::cout << "| " << y;
		std::cout << "" << std::endl;
		std::cout << std::endl;
	}
}