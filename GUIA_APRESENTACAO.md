# Guia de apresentação — Mini Metaverso Experimental

## Demonstração

1. Execute `python server.py` e abra `http://localhost:5000/cv3d`.
2. Se estiver usando o celular, acesse o endereço HTTPS publicado ou o endereço da máquina na mesma rede.
3. Mantenha o pipeline **Rostos/Avatares**, informe os nomes separados por vírgula e toque em **Câmera**.
4. Para reconhecer nomes, mantenha uma pessoa sozinha no quadro, escreva o nome e clique cinco vezes em **Cadastrar rosto**, mudando levemente o ângulo a cada amostra.
5. Mostre duas ou mais pessoas, cruze suas posições e observe que os IDs seguem a trajetória.
6. Movimente uma pessoa para os lados e aproxime-a da câmera para demonstrar X e Z.
7. Ative **Broadcast** em um aparelho, abra a mesma página em outro e clique em **Entrar em VR** para representar a visão do professor.
8. Calibre a distância, registre os testes de acessórios e exporte o CSV das medições reais.

## Fala sugerida

“A câmera fornece uma imagem composta por pixels. O OpenCV converte o frame para tons de cinza e o classificador Haar procura padrões de rosto. Para cada detecção, o sistema obtém um retângulo com posição e tamanho. O centro desse retângulo é normalizado e convertido nas coordenadas X e Y da cena A-Frame. Como uma câmera comum não mede distância diretamente, usamos o tamanho aparente do rosto para estimar a profundidade Z e permitimos calibrar a medida em centímetros. O reconhecimento de nomes usa o algoritmo LBPH treinado com amostras cadastradas. Um rastreador com previsão de movimento mantém o ID pela trajetória, inclusive quando as pessoas trocam de lado. Os frames são enviados pelo Socket.IO e o observador pode entrar em VR pelo WebXR.”

## Respostas para a investigação obrigatória

- **Como o sistema sabe onde está cada pessoa?** Pelo retângulo detectado ao redor do rosto. O centro fornece a posição 2D e um rastreador usa posição anterior e velocidade para manter o ID.
- **Como a coordenada da câmera vira posição no VR?** As coordenadas em pixels são normalizadas para uma faixa de 0 a 1 e depois convertidas para a escala da cena 3D.
- **Qual é a dificuldade da profundidade?** Uma câmera RGB produz imagem 2D e não mede distância real. O projeto usa o tamanho do rosto como aproximação, sujeita a diferenças físicas e perspectiva.
- **Pixel, coordenada e objeto 3D:** pixel é uma amostra de cor; coordenada indica uma posição; objeto 3D possui geometria e posição X, Y e Z.
- **Pessoas distantes:** podem deixar de ser detectadas quando o rosto ocupa poucos pixels. Antes disso, o rosto menor gera um Z mais distante.
- **Óculos, máscara e boné:** podem reduzir a precisão quando escondem características. Óculos comuns tendem a interferir pouco; óculos escuros, máscaras e bonés baixos interferem mais.
- **Movimento e atraso:** o painel mede a latência real de ida, processamento e retorno; o controle de FPS reduz a sobrecarga.
- **Múltiplas pessoas:** o painel registra automaticamente a maior quantidade simultânea realmente detectada no teste.
- **Sincronização:** a cena é recalculada a cada resultado recebido. O modo Broadcast retransmite a geometria para outro cliente conectado.

## Limitações

- O reconhecimento LBPH é facial, porém acadêmico: precisa de cerca de cinco amostras por pessoa e é menos robusto que modelos modernos de embeddings.
- A profundidade é estimada e não substitui um sensor LiDAR, câmera estéreo ou ARCore/ARKit.
- Iluminação ruim, rosto inclinado e oclusões podem causar perda temporária da detecção.

## Evidências experimentais

O painel **Medições reais** evita apresentar números inventados. Durante a demonstração:

1. calibre com uma pessoa a uma distância conhecida;
2. registre os testes sem acessório, com óculos escuros, máscara e boné;
3. coloque pessoas no quadro até descobrir o máximo detectado;
4. movimente-as para observar a latência média;
5. exporte `medicoes-cv3d.csv` e entregue o arquivo como evidência.
