<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Verificador e Gerador de Senhas Seguras</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Awesome para ícones -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
        }
        /* Transições suaves para a barra de progresso e ícones */
        .strength-bar {
            transition: width 0.4s ease-in-out, background-color 0.4s ease-in-out;
        }
        .check-item {
            transition: all 0.3s ease;
        }
    </style>
</head>
<body class="bg-slate-900 text-slate-100 min-h-screen flex items-center justify-center p-4 md:p-8">

    <div class="max-w-2xl w-full bg-slate-800 border border-slate-700 rounded-2xl shadow-2xl overflow-hidden my-auto">
        <!-- Cabeçalho -->
        <div class="bg-slate-850 p-6 md:p-8 border-b border-slate-700 text-center relative">
            <div class="inline-flex items-center justify-center w-14 h-14 bg-indigo-600/20 text-indigo-400 rounded-2xl mb-4 border border-indigo-500/30">
                <i class="fa-solid fa-shield-halved text-2xl"></i>
            </div>
            <h1 class="text-2xl md:text-3xl font-bold text-white tracking-tight">Segurança de Senha</h1>
            <p class="text-slate-400 text-sm md:text-base mt-2">Analise a força da sua senha em tempo real ou gere uma senha ultrassegura.</p>
        </div>

        <!-- Conteúdo Principal -->
        <div class="p-6 md:p-8 space-y-8">

            <!-- SEÇÃO 1: Entrada de Senha -->
            <div class="space-y-3">
                <label for="password-input" class="block text-sm font-semibold text-slate-300">
                    Digite sua senha para analisar:
                </label>
                <div class="relative">
                    <input type="password" id="password-input" placeholder="Digite uma senha aqui..." 
                           class="w-full bg-slate-900 border border-slate-700 text-slate-100 placeholder-slate-500 rounded-xl py-3.5 pl-4 pr-12 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent text-base transition-all">
                    <button type="button" id="toggle-password" class="absolute right-3 top-1/2 -translate-y-1/2 text-slate-400 hover:text-slate-200 p-2 rounded-lg transition-colors focus:outline-none" title="Mostrar/Ocultar Senha">
                        <i class="fa-regular fa-eye text-lg"></i>
                    </button>
                </div>

                <!-- Barra de Força e Indicador Visual -->
                <div class="pt-2 space-y-2">
                    <div class="flex justify-between items-center text-sm font-medium">
                        <span class="text-slate-400">Força da Senha:</span>
                        <span id="strength-label" class="text-slate-500 font-bold">Aguardando entrada...</span>
                    </div>
                    <div class="w-full bg-slate-900 h-3 rounded-full overflow-hidden p-0.5 border border-slate-700/50">
                        <div id="strength-bar" class="strength-bar h-full w-0 rounded-full bg-slate-600"></div>
                    </div>
                </div>
            </div>

            <!-- SEÇÃO 2: Critérios de Validação -->
            <div class="bg-slate-900/60 rounded-xl p-5 border border-slate-700/60 space-y-3">
                <h3 class="text-xs font-bold uppercase tracking-wider text-slate-400 mb-2">Requisitos de Segurança</h3>
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 text-sm">
                    <div id="check-length" class="check-item flex items-center gap-2.5 text-slate-500">
                        <i class="fa-solid fa-circle-xmark text-red-500 text-base"></i>
                        <span>Pelo menos 8 caracteres</span>
                    </div>
                    <div id="check-casing" class="check-item flex items-center gap-2.5 text-slate-500">
                        <i class="fa-solid fa-circle-xmark text-red-500 text-base"></i>
                        <span>Maiúsculas e minúsculas</span>
                    </div>
                    <div id="check-number" class="check-item flex items-center gap-2.5 text-slate-500">
                        <i class="fa-solid fa-circle-xmark text-red-500 text-base"></i>
                        <span>Pelo menos um número</span>
                    </div>
                    <div id="check-special" class="check-item flex items-center gap-2.5 text-slate-500">
                        <i class="fa-solid fa-circle-xmark text-red-500 text-base"></i>
                        <span>Caractere especial (!@#$%...)</span>
                    </div>
                    <div id="check-pattern" class="check-item flex items-center gap-2.5 text-slate-500 sm:col-span-2">
                        <i class="fa-solid fa-circle-xmark text-red-500 text-base"></i>
                        <span>Sem sequências ou padrões óbvios (ex: 12345, abc)</span>
                    </div>
                </div>
            </div>

            <!-- SEÇÃO 3: Sugestões Dinâmicas -->
            <div id="suggestions-container" class="bg-amber-500/10 border border-amber-500/30 rounded-xl p-4 hidden">
                <div class="flex items-start gap-3">
                    <i class="fa-solid fa-lightbulb text-amber-400 text-lg mt-0.5"></i>
                    <div>
                        <h4 class="text-sm font-semibold text-amber-300">Dicas para melhorar sua senha:</h4>
                        <ul id="suggestions-list" class="list-disc list-inside text-xs md:text-sm text-amber-200/80 mt-1 space-y-1">
                            <!-- Inserido dinamicamente via JS -->
                        </ul>
                    </div>
                </div>
            </div>

            <hr class="border-slate-700/80 my-6">

            <!-- SEÇÃO 4: Gerador de Senhas Integrado -->
            <div class="space-y-4">
                <div class="flex items-center justify-between">
                    <div>
                        <h3 class="text-base font-semibold text-slate-200">Gerador de Senha Forte</h3>
                        <p class="text-xs text-slate-400">Crie uma senha aleatória instantaneamente</p>
                    </div>
                    <button id="btn-generate" class="px-4 py-2 bg-indigo-600 hover:bg-indigo-500 text-white text-sm font-medium rounded-lg transition-colors shadow-lg shadow-indigo-600/30 flex items-center gap-2">
                        <i class="fa-solid fa-wand-magic-sparkles"></i> Gerar
                    </button>
                </div>

                <!-- Campo do Resultado Gerado -->
                <div class="flex gap-2">
                    <div class="relative flex-1">
                        <input type="text" id="generated-password" readonly placeholder="Clique em Gerar..." 
                               class="w-full bg-slate-900 border border-slate-700 text-emerald-400 font-mono font-medium rounded-xl py-3 px-4 focus:outline-none text-base">
                    </div>
                    <button id="btn-copy" class="px-4 py-3 bg-slate-700 hover:bg-slate-600 active:bg-slate-800 text-slate-200 font-medium rounded-xl transition-colors flex items-center justify-center gap-2 min-w-[100px]" title="Copiar para área de transferência">
                        <i class="fa-regular fa-copy"></i>
                        <span id="copy-text">Copiar</span>
                    </button>
                </div>

                <!-- Controle do Tamanho da Senha -->
                <div class="flex items-center gap-4 pt-1">
                    <label for="length-range" class="text-xs text-slate-400 shrink-0">Tamanho da senha:</label>
                    <input type="range" id="length-range" min="8" max="32" value="16" class="w-full accent-indigo-500 bg-slate-700 h-1.5 rounded-lg appearance-none cursor-pointer">
                    <span id="length-value" class="text-sm font-bold text-indigo-400 font-mono w-6 text-right">16</span>
                </div>
            </div>

        </div>

        <!-- Rodapé da Aplicação -->
        <div class="bg-slate-850 px-6 py-4 border-t border-slate-700/60 text-center text-xs text-slate-500">
            <p>Suas senhas são processadas 100% localmente no seu navegador e nunca são enviadas para nenhum servidor.</p>
        </div>
    </div>

    <!-- Caixa de Notificação Toast de Cópia -->
    <div id="toast" class="fixed bottom-5 right-5 bg-emerald-600 text-white px-4 py-3 rounded-xl shadow-xl flex items-center gap-3 transition-opacity duration-300 opacity-0 pointer-events-none">
        <i class="fa-solid fa-circle-check text-lg"></i>
        <span class="text-sm font-medium">Senha copiada para a área de transferência!</span>
    </div>

    <!-- Script com toda a lógica JavaScript -->
    <script>
        // Elementos DOM principais
        const passwordInput = document.getElementById('password-input');
        const togglePasswordBtn = document.getElementById('toggle-password');
        const strengthBar = document.getElementById('strength-bar');
        const strengthLabel = document.getElementById('strength-label');
        const suggestionsContainer = document.getElementById('suggestions-container');
        const suggestionsList = document.getElementById('suggestions-list');

        // Elementos dos Critérios
        const checkLength = document.getElementById('check-length');
        const checkCasing = document.getElementById('check-casing');
        const checkNumber = document.getElementById('check-number');
        const checkSpecial = document.getElementById('check-special');
        const checkPattern = document.getElementById('check-pattern');

        // Elementos do Gerador
        const btnGenerate = document.getElementById('btn-generate');
        const generatedPasswordInput = document.getElementById('generated-password');
        const btnCopy = document.getElementById('btn-copy');
        const copyText = document.getElementById('copy-text');
        const lengthRange = document.getElementById('length-range');
        const lengthValue = document.getElementById('length-value');
        const toast = document.getElementById('toast');

        // Lista de padrões e sequências comuns a serem evitados
        const commonPatterns = [
            '123456', '12345678', '123456789', 'qwerty', 'password', 'senha', 
            '111111', 'abc123', '000000', 'admin', 'welcome', 'asdfgh'
        ];

        // Alternar Visibilidade da Senha
        togglePasswordBtn.addEventListener('click', () => {
            const isPassword = passwordInput.getAttribute('type') === 'password';
            passwordInput.setAttribute('type', isPassword ? 'text' : 'password');
            
            const icon = togglePasswordBtn.querySelector('i');
            if (isPassword) {
                icon.className = 'fa-regular fa-eye-slash text-indigo-400';
            } else {
                icon.className = 'fa-regular fa-eye text-slate-400';
            }
        });

        // Função para atualizar o status do item de checklist
        function updateCheckItem(element, isValid) {
            const icon = element.querySelector('i');
            if (isValid) {
                element.className = 'check-item flex items-center gap-2.5 text-slate-200 font-medium';
                icon.className = 'fa-solid fa-circle-check text-emerald-400 text-base';
            } else {
                element.className = 'check-item flex items-center gap-2.5 text-slate-500';
                icon.className = 'fa-solid fa-circle-xmark text-red-500/80 text-base';
            }
        }

        // Analisador Principal de Senha
        function analyzePassword() {
            const val = passwordInput.value;
            const suggestions = [];

            if (val.length === 0) {
                // Estado Vazio
                strengthBar.style.width = '0%';
                strengthBar.className = 'strength-bar h-full rounded-full bg-slate-600';
                strengthLabel.textContent = 'Aguardando entrada...';
                strengthLabel.className = 'text-slate-500 font-bold';
                
                [checkLength, checkCasing, checkNumber, checkSpecial, checkPattern].forEach(el => updateCheckItem(el, false));
                suggestionsContainer.classList.add('hidden');
                return;
            }

            // Testar critérios individuais
            const hasMinLength = val.length >= 8;
            const hasUpperAndLower = /[a-z]/.test(val) && /[A-Z]/.test(val);
            const hasNumber = /[0-9]/.test(val);
            const hasSpecial = /[!@#$%^&*()_+\-=\[\]{};':"\\|,.<>\/?]/.test(val);
            
            // Verificar ausência de sequências ou padrões óbvios
            const lowerVal = val.toLowerCase();
            const hasCommonPattern = commonPatterns.some(pattern => lowerVal.includes(pattern));
            const hasRepeatingChars = /(.)\1{2,}/.test(val); // Ex: "aaa" ou "111"
            const isPatternSafe = !hasCommonPattern && !hasRepeatingChars;

            // Atualizar os ícones de checklist
            updateCheckItem(checkLength, hasMinLength);
            updateCheckItem(checkCasing, hasUpperAndLower);
            updateCheckItem(checkNumber, hasNumber);
            updateCheckItem(checkSpecial, hasSpecial);
            updateCheckItem(checkPattern, isPatternSafe);

            // Coletar sugestões
            if (!hasMinLength) suggestions.push('Aumente o tamanho para no mínimo 8 caracteres (idealmente 12+).');
            if (!hasUpperAndLower) suggestions.push('Misture letras MAIÚSCULAS e minúsculas.');
            if (!hasNumber) suggestions.push('Adicione pelo menos um número (0-9).');
            if (!hasSpecial) suggestions.push('Inclua símbolos especiais como @, #, $, %, ! ou *.');
            if (hasCommonPattern) suggestions.push('Evite palavras ou sequências comuns como "123456" ou "qwerty".');
            if (hasRepeatingChars) suggestions.push('Evite repetir o mesmo caractere várias vezes seguidas.');

            // Calcular pontuação de força (0 a 100)
            let score = 0;
            if (val.length >= 8) score += 20;
            if (val.length >= 12) score += 15;
            if (val.length >= 16) score += 10;
            if (hasUpperAndLower) score += 20;
            if (hasNumber) score += 15;
            if (hasSpecial) score += 20;
            if (isPatternSafe) score += 10; else score = Math.max(0, score - 20);

            // Exibir Nível de Força com base na pontuação
            if (score < 40) {
                strengthBar.style.width = '25%';
                strengthBar.className = 'strength-bar h-full rounded-full bg-red-500';
                strengthLabel.textContent = 'Fraca';
                strengthLabel.className = 'text-red-400 font-bold';
            } else if (score < 70) {
                strengthBar.style.width = '55%';
                strengthBar.className = 'strength-bar h-full rounded-full bg-amber-500';
                strengthLabel.textContent = 'Média';
                strengthLabel.className = 'text-amber-400 font-bold';
            } else if (score < 90) {
                strengthBar.style.width = '80%';
                strengthBar.className = 'strength-bar h-full rounded-full bg-blue-500';
                strengthLabel.textContent = 'Forte';
                strengthLabel.className = 'text-blue-400 font-bold';
            } else {
                strengthBar.style.width = '100%';
                strengthBar.className = 'strength-bar h-full rounded-full bg-emerald-500';
                strengthLabel.textContent = 'Muito Forte';
                strengthLabel.className = 'text-emerald-400 font-bold';
            }

            // Exibir ou ocultar sugestões
            if (suggestions.length > 0) {
                suggestionsList.innerHTML = suggestions.map(s => `<li>${s}</li>`).join('');
                suggestionsContainer.classList.remove('hidden');
            } else {
                suggestionsContainer.classList.add('hidden');
            }
        }

        // Listener no input de senha
        passwordInput.addEventListener('input', analyzePassword);

        // Funções do Gerador de Senhas
        function generateStrongPassword(length = 16) {
            const uppers = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
            const lowers = 'abcdefghijklmnopqrstuvwxyz';
            const numbers = '0123456789';
            const symbols = '!@#$%^&*()_+-=[]{}|;:,.<>?';
            const allChars = uppers + lowers + numbers + symbols;

            let result = '';
            // Garantir que pelo menos um caractere de cada tipo esteja presente
            result += uppers.charAt(Math.floor(Math.random() * uppers.length));
            result += lowers.charAt(Math.floor(Math.random() * lowers.length));
            result += numbers.charAt(Math.floor(Math.random() * numbers.length));
            result += symbols.charAt(Math.floor(Math.random() * symbols.length));

            // Preencher o restante
            for (let i = 4; i < length; i++) {
                result += allChars.charAt(Math.floor(Math.random() * allChars.length));
            }

            // Embaralhar o resultado para não manter a ordem dos primeiros caracteres fixos
            return result.split('').sort(() => 0.5 - Math.random()).join('');
        }

        // Evento do botão de gerar
        btnGenerate.addEventListener('click', () => {
            const length = parseInt(lengthRange.value, 10);
            const newPassword = generateStrongPassword(length);
            generatedPasswordInput.value = newPassword;
            
            // Opcional: preencher o verificador para testar a senha gerada
            passwordInput.value = newPassword;
            analyzePassword();
        });

        // Atualizar tamanho do slider
        lengthRange.addEventListener('input', (e) => {
            lengthValue.textContent = e.target.value;
        });

        // Copiar para a Área de Transferência (Compatível com Iframe/Web)
        btnCopy.addEventListener('click', () => {
            const textToCopy = generatedPasswordInput.value;
            if (!textToCopy) return;

            // Método fallback usando execCommand para maior compatibilidade
            const tempInput = document.createElement('input');
            tempInput.value = textToCopy;
            document.body.appendChild(tempInput);
            tempInput.select();
            
            try {
                document.execCommand('copy');
                
                // Animação/Feedback no Botão
                copyText.textContent = 'Copiado!';
                btnCopy.classList.remove('bg-slate-700', 'hover:bg-slate-600');
                btnCopy.classList.add('bg-emerald-600', 'text-white');

                // Notificação Toast
                toast.classList.remove('opacity-0', 'pointer-events-none');
                toast.classList.add('opacity-100');

                setTimeout(() => {
                    copyText.textContent = 'Copiar';
                    btnCopy.classList.remove('bg-emerald-600', 'text-white');
                    btnCopy.classList.add('bg-slate-700', 'hover:bg-slate-600');
                    toast.classList.remove('opacity-100');
                    toast.classList.add('opacity-0', 'pointer-events-none');
                }, 2000);

            } catch (err) {
                console.error('Falha ao copiar:', err);
            } finally {
                document.body.removeChild(tempInput);
            }
        });

        // Gerar uma senha automaticamente no carregamento inicial da página
        window.onload = () => {
            btnGenerate.click();
        };
    </script>
</body>
</html>
