# Correções realizadas após a revisão

| Ponto levantado | Correção implementada | Como comprovar |
|---|---|---|
| Ambiente integrado utilizava Three.js | A cena de `/cv3d` foi reconstruída com A-Frame 1.6 | Inspecionar `templates/cv3d.html` e `static/js/cv3d-aframe.js` |
| Observador não tinha VR/WebXR | A cena possui `vr-mode-ui`, câmera A-Frame e botão **Entrar em VR** | Abrir um cliente observador e entrar em VR |
| Nomes não eram reconhecimento facial | Foi incluído cadastro de amostras e reconhecimento LBPH do OpenCV | Cadastrar cinco amostras e voltar ao enquadramento |
| IDs trocavam quando pessoas mudavam de lado | O servidor usa rastreamento temporal com posição, tamanho e velocidade prevista | Fazer duas pessoas cruzarem suas posições |
| Investigação sem medições | O painel mede latência, distância calibrada, máximo de pessoas e resultados com acessórios | Realizar testes e exportar `medicoes-cv3d.csv` |

## Observação técnica

O rastreamento reduz trocas de ID, mas nenhuma abordagem baseada somente em
detecção 2D garante 100% de estabilidade quando um rosto fica totalmente oculto.
O reconhecimento facial ajuda a recuperar a identidade quando a pessoa já foi
cadastrada.
