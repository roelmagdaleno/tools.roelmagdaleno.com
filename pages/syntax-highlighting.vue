<template>
	<div>
		<p class="text-gray-500">
			Syntax highlight your source code by specifying the programming language and theme.
		</p>

		<p class="text-gray-500">
			Powered by <a href="https://torchlight.dev/" class="link" target="_blank">Torchlight</a>.
		</p>

		<div class="mt-8 mb-16">
			<div class="grid grid-cols-1 gap-y-6 gap-x-4 sm:grid-cols-6">
				<div class="sm:col-span-3">
					<Listbox as="div" v-model="language">
						<ListboxLabel class="block text-sm font-medium text-gray-700">
							Language
						</ListboxLabel>
						<div class="mt-1 relative">
							<ListboxButton class="bg-white relative w-full border border-gray-300 rounded-md shadow-sm pl-3 pr-10 py-2 text-left cursor-default focus:outline-none focus:ring-1 focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
								<span class="block truncate">{{ language.name }}</span>
								<span class="absolute inset-y-0 right-0 flex items-center pr-2 pointer-events-none">
									<SelectorIcon class="h-5 w-5 text-gray-400" aria-hidden="true" />
								</span>
							</ListboxButton>

							<transition leave-active-class="transition ease-in duration-100" leave-from-class="opacity-100" leave-to-class="opacity-0">
								<ListboxOptions class="absolute z-10 mt-1 w-full bg-white shadow-lg max-h-60 rounded-md py-1 text-base ring-1 ring-black ring-opacity-5 overflow-auto focus:outline-none sm:text-sm">
									<ListboxOption as="template" v-for="option in languages" :key="option.id" :value="option" v-slot="{ active, selected }">
										<li :class="[active ? 'text-white bg-indigo-600' : 'text-gray-900', 'cursor-default select-none relative py-2 pl-3 pr-9']">
							<span :class="[selected ? 'font-semibold' : 'font-normal', 'block truncate']">
								{{ option.name }}
							</span>

											<span v-if="selected" :class="[active ? 'text-white' : 'text-indigo-600', 'absolute inset-y-0 right-0 flex items-center pr-4']">
								<CheckIcon class="h-5 w-5" aria-hidden="true" />
							</span>
										</li>
									</ListboxOption>
								</ListboxOptions>
							</transition>
						</div>
					</Listbox>
				</div>
				<div class="sm:col-span-3">
					<Listbox as="div" v-model="theme">
						<ListboxLabel class="block text-sm font-medium text-gray-700">
							Theme
						</ListboxLabel>
						<div class="mt-1 relative">
							<ListboxButton class="bg-white relative w-full border border-gray-300 rounded-md shadow-sm pl-3 pr-10 py-2 text-left cursor-default focus:outline-none focus:ring-1 focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
								<span class="block truncate">{{ theme.name }}</span>
								<span class="absolute inset-y-0 right-0 flex items-center pr-2 pointer-events-none">
									<SelectorIcon class="h-5 w-5 text-gray-400" aria-hidden="true" />
								</span>
							</ListboxButton>

							<transition leave-active-class="transition ease-in duration-100" leave-from-class="opacity-100" leave-to-class="opacity-0">
								<ListboxOptions class="absolute z-10 mt-1 w-full bg-white shadow-lg max-h-60 rounded-md py-1 text-base ring-1 ring-black ring-opacity-5 overflow-auto focus:outline-none sm:text-sm">
									<ListboxOption as="template" v-for="option in themes" :key="option.id" :value="option" v-slot="{ active, selected }">
										<li :class="[active ? 'text-white bg-indigo-600' : 'text-gray-900', 'cursor-default select-none relative py-2 pl-3 pr-9']">
							<span :class="[selected ? 'font-semibold' : 'font-normal', 'block truncate']">
								{{ option.name }}
							</span>

											<span v-if="selected" :class="[active ? 'text-white' : 'text-indigo-600', 'absolute inset-y-0 right-0 flex items-center pr-4']">
								<CheckIcon class="h-5 w-5" aria-hidden="true" />
							</span>
										</li>
									</ListboxOption>
								</ListboxOptions>
							</transition>
						</div>
					</Listbox>
				</div>
			</div>

			<div class="options mt-4 md:flex items-center justify-end">
				<div class="flex items-center mr-4">
					<label for="line-numbers-start" class="block text-sm font-medium text-gray-700 mr-3">
						Number of the first line:
					</label>
					<div class="mt-1">
						<input type="number" id="line-numbers-start" class="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md w-20" v-model="lineNumbersStart" />
					</div>
				</div>
				<div class="mt-4 md:mt-1 md:flex">
					<div class="mb-4 md:mr-4 md:mb-0">
						<SwitchGroup as="div" class="flex items-center">
							<Switch v-model="lineNumbers" :class="[lineNumbers ? 'bg-indigo-600' : 'bg-gray-200', 'relative inline-flex flex-shrink-0 h-6 w-11 border-2 border-transparent rounded-full cursor-pointer transition-colors ease-in-out duration-200 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500']">
								<span aria-hidden="true" :class="[lineNumbers ? 'translate-x-5' : 'translate-x-0', 'pointer-events-none inline-block h-5 w-5 rounded-full bg-white shadow transform ring-0 transition ease-in-out duration-200']" />
							</Switch>
							<SwitchLabel as="span" class="ml-3">
								<span class="text-sm font-medium text-gray-900">Line Numbers </span>
							</SwitchLabel>
						</SwitchGroup>
					</div>

					<div>
						<SwitchGroup as="div" class="flex items-center">
							<Switch v-model="diffIndicators" :class="[diffIndicators ? 'bg-indigo-600' : 'bg-gray-200', 'relative inline-flex flex-shrink-0 h-6 w-11 border-2 border-transparent rounded-full cursor-pointer transition-colors ease-in-out duration-200 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500']">
								<span aria-hidden="true" :class="[diffIndicators ? 'translate-x-5' : 'translate-x-0', 'pointer-events-none inline-block h-5 w-5 rounded-full bg-white shadow transform ring-0 transition ease-in-out duration-200']" />
							</Switch>
							<SwitchLabel as="span" class="ml-3">
								<span class="text-sm font-medium text-gray-900">Diff Indicator </span>
								<span class="text-sm text-gray-500">(`+` / `-`)</span>
							</SwitchLabel>
						</SwitchGroup>
					</div>
				</div>
			</div>

			<div>
				<div class="mt-8 grid grid-cols-1 gap-y-6 gap-x-4 sm:grid-cols-2">
					<div>
						<label
							for="code-input"
							class="block text-sm font-medium text-gray-700"
						>
							Code
						</label>

						<div class="mt-4">
							<textarea
								rows="25"
								name="code-input"
								id="code-input"
								v-model.trim="codeInput"
								class="shadow-sm focus:ring-indigo-500 focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md textarea-code"
								autocomplete="off"
								spellcheck="false"
								ref="textareaCodeInput"
								required
							/>
						</div>
					</div>

					<div>
						<p class="text-sm font-medium text-gray-700 inline-flex">
							<span>Output</span>
							<svg xmlns="http://www.w3.org/2000/svg" ref="copyFAQ" class="ml-2 h-5 w-5" viewBox="0 0 20 20" fill="currentColor"><path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-8-3a1 1 0 00-.867.5 1 1 0 11-1.731-1A3 3 0 0113 8a3.001 3.001 0 01-2 2.83V11a1 1 0 11-2 0v-1a1 1 0 011-1 1 1 0 100-2zm0 8a1 1 0 100-2 1 1 0 000 2z" clip-rule="evenodd" /></svg>
						</p>

						<div class="mt-4">
							<CodeBlock
								:syntaxHighlighted="syntaxHighlighted"
								:styles="styles"
								copyOption="html"
							/>
						</div>
					</div>
				</div>
			</div>
		</div>

		<div class="flex sticky bg-white w-full p-4 rounded-lg shadow bottom-4">
			<Button type="button" action="secondary" class="mr-4" @click="clearInput">
				Clear
			</Button>

			<Button type="button" action="primary" :loading="loading" @click="syntaxHighlight">
				Syntax Highlight
			</Button>
		</div>
	</div>
</template>

<script setup>
import tippy from 'tippy.js';
import 'tippy.js/dist/tippy.css';
import autosize from 'autosize';
import { defaultSyntaxHighlighted, defaultCodeInput } from '../assets/js/defaults.js';
import { disableElements } from "../assets/js/disableElements";
import {
	Switch,
	SwitchGroup,
	SwitchLabel,
	Listbox,
	ListboxButton,
	ListboxLabel,
	ListboxOption,
	ListboxOptions
} from '@headlessui/vue';
import { CheckIcon, SelectorIcon } from '@heroicons/vue/solid';

useHead({
	title: 'Syntax Highlighting',
	meta: [
		{ name: 'description', content: 'Syntax highlight your source code by specifying the programming language and theme. Powered by Torchlight.'},
		{ name: 'keywords', content: 'Syntax Highlighting, Torchlight' },
		{ property: 'og:type', content: 'website' },
		{ property: 'og:title', content: 'Syntax Highlighting - DevTools by Roel Magdaleno' },
		{ property: 'og:image', content: 'https://devtools.roelmagdaleno.com/img/syntax-highlighting.png' },
		{ property: 'og:url', content: 'https://devtools.roelmagdaleno.com/syntax-highlighting/' },
		{ property: 'og:description', content: 'Syntax highlight your source code by specifying the programming language and theme. Powered by Torchlight.' },
		{ property: 'twitter:title', content: 'Syntax Highlighting - DevTools by Roel Magdaleno' },
		{ property: 'twitter:description', content: 'Syntax highlight your source code by specifying the programming language and theme. Powered by Torchlight.' },
		{ property: 'twitter:card', content: 'summary_large_image' },
		{ property: 'twitter:image', content: 'https://devtools.roelmagdaleno.com/img/syntax-highlighting.png' },
		{ property: 'twitter:site', content: '@roelmagdaleno' },
		{ property: 'twitter:creator', content: '@roelmagdaleno' },
		{ property: 'twitter:url', content: 'https://devtools.roelmagdaleno.com/syntax-highlighting/' },
	],
});

const languages = [
	{ id: '(null) | txt | text | plaintext', name: 'Plaintext' },
	{ id: 'abap', name: 'ABAP' },
	{ id: 'actionscript-3', name: 'Actionscript 3' },
	{ id: 'ada', name: 'Ada' },
	{ id: 'alpine | alpinejs', name: 'Alpine.js' },
	{ id: 'antlers', name: 'Statamic Antlers' },
	{ id: 'apex', name: 'Apex' },
	{ id: 'applescript', name: 'Applescript' },
	{ id: 'asm', name: 'Assembly' },
	{ id: 'awk', name: 'AWK' },
	{ id: 'bat | batch', name: 'Batch' },
	{ id: 'blade', name: 'Laravel Blade' },
	{ id: 'c', name: 'C' },
	{ id: 'clojure | clj', name: 'Clojure' },
	{ id: 'cobol', name: 'Cobol' },
	{ id: 'coffee', name: 'Coffee Script' },
	{ id: 'cpp', name: 'C++' },
	{ id: 'crystal', name: 'Crystal' },
	{ id: 'csharp | c#', name: 'C#' },
	{ id: 'css', name: 'CSS' },
	{ id: 'curl', name: 'Curl' },
	{ id: 'd', name: 'D' },
	{ id: 'dart', name: 'Dart' },
	{ id: 'diff', name: 'git-diff' },
	{ id: 'dockerfile', name: 'Dockerfile' },
	{ id: 'elixir', name: 'Elixir' },
	{ id: 'elm', name: 'Elm' },
	{ id: 'erlang', name: 'Erlang' },
	{ id: 'fsharp | f#', name: 'F#' },
	{ id: 'git-commit', name: 'git-commit' },
	{ id: 'git-ignore | gitignore', name: 'git-ignore' },
	{ id: 'git-rebase', name: 'git-rebase' },
	{ id: 'gnuplot', name: 'gnuplot' },
	{ id: 'go', name: 'Go' },
	{ id: 'graphql', name: 'GraphQL' },
	{ id: 'groovy', name: 'Groovy' },
	{ id: 'hack', name: 'Hack' },
	{ id: 'haml', name: 'Haml' },
	{ id: 'handlebars | hbs', name: 'Handlebars' },
	{ id: 'haskell', name: 'Haskell' },
	{ id: 'hcl', name: 'HashiCorp Configuration Language' },
	{ id: 'hlsl', name: 'HLSL' },
	{ id: 'html', name: 'HTML' },
	{ id: 'html-ruby-erb | erb', name: 'Embedded Ruby (ERB)' },
	{ id: 'ini', name: 'INI' },
	{ id: 'java', name: 'Java' },
	{ id: 'javascript | js', name: 'JavaScript' },
	{ id: 'jinja-html', name: 'Jinja HTML' },
	{ id: 'json', name: 'JSON' },
	{ id: 'jsonc', name: 'jsonc' },
	{ id: 'jsonnet', name: 'Jsonnet' },
	{ id: 'jsx', name: 'JSX' },
	{ id: 'julia', name: 'Julia' },
	{ id: 'kotlin', name: 'Kotlin' },
	{ id: 'latex | tex', name: 'Latex' },
	{ id: 'less', name: 'Less' },
	{ id: 'lisp', name: 'Lisp' },
	{ id: 'logo', name: 'Logo' },
	{ id: 'lua', name: 'Lua' },
	{ id: 'makefile', name: 'Makefile' },
	{ id: 'markdown | md', name: 'Markdown' },
	{ id: 'matlab', name: 'MATLAB' },
	{ id: 'mdx', name: 'MDX' },
	{ id: 'nix', name: 'Nix' },
	{ id: 'objective-c | objc', name: 'Objective-C' },
	{ id: 'ocaml', name: 'OCaml' },
	{ id: 'pascal', name: 'Pascal' },
	{ id: 'perl', name: 'Perl' },
	{ id: 'perl6', name: 'Perl6' },
	{ id: 'php', name: 'PHP' },
	{ id: 'pls', name: 'PLSQL' },
	{ id: 'postcss', name: 'PostCSS' },
	{ id: 'powershell | ps | ps1', name: 'PowerShell' },
	{ id: 'prolog', name: 'Prolog' },
	{ id: 'pug | jade', name: 'Pug' },
	{ id: 'puppet', name: 'Puppet' },
	{ id: 'purescript', name: 'PureScript' },
	{ id: 'python | py', name: 'Python' },
	{ id: 'r', name: 'R' },
	{ id: 'razor', name: 'Razor' },
	{ id: 'ruby | rb', name: 'Ruby' },
	{ id: 'rust', name: 'Rust' },
	{ id: 'sas', name: 'SAS' },
	{ id: 'sass', name: 'Sass' },
	{ id: 'scala', name: 'Scala' },
	{ id: 'scheme', name: 'Scheme' },
	{ id: 'scss', name: 'SCSS' },
	{ id: 'shaderlab | shader', name: 'ShaderLab' },
	{ id: 'shell', name: 'Shell' },
	{ id: 'smalltalk', name: 'Smalltalk' },
	{ id: 'sql', name: 'SQL' },
	{ id: 'ssh-config', name: 'ssh-config' },
	{ id: 'stylus | styl', name: 'Stylus' },
	{ id: 'swift', name: 'Swift' },
	{ id: 'tcl', name: 'Tcl' },
	{ id: 'toml', name: 'TOML' },
	{ id: 'tsx', name: 'tsx' },
	{ id: 'typescript | ts', name: 'TypeScript' },
	{ id: 'vb | cmd', name: 'VB' },
	{ id: 'viml', name: 'VimL' },
	{ id: 'vue', name: 'Vue' },
	{ id: 'wasm', name: 'WebAssembly' },
	{ id: 'xml', name: 'XML' },
	{ id: 'xsl', name: 'XSL' },
	{ id: 'yaml', name: 'YAML' },
	{ id: '文言 | wenyan', name: 'Wenyan' },
];

const themes = [
	{ id: 'atom-one-dark', name: 'Atom One Dark' },
	{ id: 'cobalt2', name: 'Cobalt2' },
	{ id: 'dark-404', name: 'Dark 404' },
	{ id: 'dark-plus', name: 'Dark Plus' },
	{ id: 'deep-purple', name: 'Deep Purple' },
	{ id: 'dracula', name: 'Dracula' },
	{ id: 'fortnite', name: 'Fortnite' },
	{ id: 'github-dark', name: 'Github Dark' },
	{ id: 'github-light', name: 'Github Light' },
	{ id: 'iceberg-dark', name: 'Iceberg Dark' },
	{ id: 'light-plus', name: 'Light Plus' },
	{ id: 'liver-dark', name: 'Liver Dark' },
	{ id: 'liver-purple', name: 'Liver Purple' },
	{ id: 'material-facebook', name: 'Material Facebook' },
	{ id: 'material-theme-darker', name: 'Material Theme Darker' },
	{ id: 'material-theme-default', name: 'Material Theme Default' },
	{ id: 'material-theme-lighter', name: 'Material Theme Lighter' },
	{ id: 'material-theme-ocean', name: 'Material Theme Ocean' },
	{ id: 'material-theme-palenight', name: 'Material Theme Palenight' },
	{ id: 'min-dark', name: 'Min Dark' },
	{ id: 'min-light', name: 'Min Light' },
	{ id: 'monokai', name: 'Monokai' },
	{ id: 'monokuro-blue', name: 'Monokuro Blue' },
	{ id: 'monokuro-brown', name: 'Monokuro Brown' },
	{ id: 'monokuro-cyan', name: 'Monokuro Cyan' },
	{ id: 'monokuro-gray', name: 'Monokuro Gray' },
	{ id: 'monokuro-green', name: 'Monokuro Green' },
	{ id: 'monokuro-indigo', name: 'Monokuro Indigo' },
	{ id: 'monokuro-lime', name: 'Monokuro Lime' },
	{ id: 'monokuro-orange', name: 'Monokuro Orange' },
	{ id: 'monokuro-pink', name: 'Monokuro Pink' },
	{ id: 'monokuro-purple', name: 'Monokuro Purple' },
	{ id: 'monokuro-red', name: 'Monokuro Red' },
	{ id: 'monokuro-teal', name: 'Monokuro Teal' },
	{ id: 'monokuro-yellow', name: 'Monokuro Yellow' },
	{ id: 'moonlight', name: 'Moonlight' },
	{ id: 'moonlight-ii', name: 'Moonlight II' },
	{ id: 'night-owl', name: 'Night Owl' },
	{ id: 'nord', name: 'Nord' },
	{ id: 'olaolu-palenight', name: 'Olaolu Olawuyi\'s Palenight' },
	{ id: 'one-dark-pro', name: 'One Dark Pro' },
	{ id: 'serendipity-dark', name: 'Serendipity Dark' },
	{ id: 'serendipity-light', name: 'Serendipity Light' },
	{ id: 'shades-of-purple', name: 'Shades of Purple' },
	{ id: 'slack-theme-dark-mode', name: 'Slack Theme Dark Mode' },
	{ id: 'slack-theme-ochin', name: 'Slack Theme Ochin' },
	{ id: 'solarized-dark', name: 'Solarized Dark' },
	{ id: 'solarized-light', name: 'Solarized Light' },
	{ id: 'synthwave-84', name: 'Synthwave 84' },
	{ id: 'winter-is-coming-blue', name: 'Winter Is Coming Blue' },
	{ id: 'winter-is-coming-dark', name: 'Winter Is Coming Dark' },
	{ id: 'winter-is-coming-light', name: 'Winter Is Coming Light' },
	{ id: 'yellow-delight', name: 'Yellow Delight' },
];

const runtimeConfig = useRuntimeConfig();
const language = ref(languages[69]);
const theme = ref(themes[7]);
const codeInput = ref(defaultCodeInput);
const textareaCodeInput = ref(null);
const syntaxHighlighted = ref(defaultSyntaxHighlighted);
const errorMessage = ref(false);
const loading = ref(false);
const lineNumbers = ref(false);
const diffIndicators = ref(false);
const lineNumbersStart = ref(1);
const styles = ref('background-color: #24292e; --theme-selection-background: #39414a;');
const copyFAQ = ref(null);

onMounted(() => {
	autosize(textareaCodeInput.value);

	tippy(copyFAQ.value, {
		content: 'If you want to render the copied HTML in your website, you must include the <a href="https://torchlight.dev/docs/css#standard-css" class="text-indigo-400" target="_blank">Torchlight CSS</a>.',
		allowHTML: true,
		interactive: true,
	});
});

watch(codeInput, () => {
	autosize(textareaCodeInput.value);
});

async function syntaxHighlight() {
	try {
		errorMessage.value = false;
		loading.value = true;

		toggleElements();

		const route = `${ runtimeConfig.public.apiBase }/wp-json/wp-devtools/v1/syntax-highlighting`;
		const options = {
			method: 'POST',
			body: {
				code: codeInput.value,
				language: language.value.id,
				theme: theme.value.id,
				options: {
					diffIndicators: diffIndicators.value,
					lineNumbers: lineNumbers.value,
					lineNumbersStart: lineNumbersStart.value,
				},
			},
			parseResponse: JSON.parse,
		};

		const response = await $fetch(route, options);
		syntaxHighlighted.value = response.syntax_highlighted;
		styles.value = response.styles;

		loading.value = false;
		toggleElements();
	} catch (e) {
		errorMessage.value = e.data.message;
		loading.value = false;
		toggleElements();
	}
}

function toggleElements() {
	const formElements = [
		document.querySelector('#code-input'),
	];

	disableElements(formElements, loading.value);
}

function clearInput() {
	codeInput.value = '';
	textareaCodeInput.value.focus();
}
</script>
