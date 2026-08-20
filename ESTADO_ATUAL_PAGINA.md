# Estrutura do Portfólio - Parosi Serviços (Atualizado)

Este documento registra o estado atual da página `index.html` para garantir que futuros agentes saibam exatamente como a estrutura foi montada, evitando perda de progresso.

## 1. Diretórios de Imagens Locais
Para evitar problemas com links quebrados do Instagram e filtros indesejados, todas as imagens foram baixadas localmente e estão organizadas nos seguintes diretórios dentro de `parosi_servico/`:
- `solucoes_customizadas/`: Contém 78 imagens de projetos de SaaS e automação.
- `toldos_e_fachadas/`: Contém 10 imagens de alta qualidade dos carrosséis de toldos.
- `soldas_especiais/`: Contém 19 imagens de trabalhos de solda.
- `manutencao_geral/`: Contém as imagens (extraídas de fotos e thumbnails de reels) de manutenção e portões.

## 2. Estrutura dos Modais no HTML
Os modais de portfólio no `index.html` (`modal-solucoes`, `modal-fachadas`, `modal-soldas` e `modal-landing`) agora utilizam um layout de carrossel deslizante (scroll horizontal com `snap-x`). 

### Padrão HTML do Carrossel:
```html
<div class="flex overflow-x-auto snap-x snap-mandatory gap-4 pb-4 custom-scrollbar">
    <!-- Bloco de Imagem -->
    <div class="snap-center shrink-0 w-[90%] md:w-[70%] lg:w-[60%] border border-white/10 bg-[#050505] p-2 hover:border-amber/50 transition-colors">
        <img loading="lazy" src="[NOME_DA_PASTA]/[NOME_DO_ARQUIVO].jpg" alt="Projeto..." class="w-full h-auto object-cover">
    </div>
</div>
```

**Regras para Futuros Agentes:**
1. **NÃO** volte a usar links diretos do Instagram (`instagram.com/...`) no src das imagens do portfólio.
2. Sempre referencie os arquivos locais baixados.
3. Mantenha as classes do Tailwind (`w-[90%] md:w-[70%] lg:w-[60%]`, `snap-center`, `custom-scrollbar`) para manter o design consistente com as Landing Pages.
4. Qualquer nova imagem deve ser salva nas pastas correspondentes e injetada na mesma estrutura de carrossel.
