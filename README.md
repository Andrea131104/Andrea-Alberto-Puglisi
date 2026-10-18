% !TeX program = xelatex
\documentclass[11pt,a4paper]{article}

\usepackage[a4paper,margin=2.5cm]{geometry}
\usepackage{amsmath}
\usepackage{amssymb}
\usepackage{tikz}
\usepackage{pgfplots}
\usepackage{cancel}

\pgfplotsset{compat=1.18}
\usetikzlibrary{positioning, shapes.geometric, arrows.meta}

\title{\textbf{Nuclear Fusion Reactor Physics}\\[0.3cm]
\large Introduction \& Global Energy Balance\\
\normalsize Politecnico di Torino -- Department of Energy ``G. Ferraris''\\
A.A. 2026/2027 -- Prof. E. Bray, Prof. F. Subba}
\author{Appunti integrati e trascrizione formale delle lezioni}
\date{}

\begin{document}


\newpage

% ============================================================================
\section{Outline del Corso}
% ============================================================================

L'analisi introduttiva e il bilancio energetico globale di un reattore a fusione nucleare si articolano nei seguenti punti principali:
\begin{enumerate}
    \item \textbf{General characterization of a thermo-nuclear fusion plasma}: caratterizzazione fisica e cinetica del plasma termonucleare.
    \item \textbf{Basic definitions}:
    \begin{enumerate}
        \item \textbf{Confinement time} ($\tau_E$): descrizione fisica del tempo caratteristico di perdita dell'energia interna.
        \item \textbf{Lawson criterion}: prospettiva ingegneristica su come il plasma viene utilizzato a fini energetici.
    \end{enumerate}
\end{enumerate}

% ============================================================================
\section{Caratterizzazione Generale di un Plasma Termonucleare}
% ============================================================================

\subsection{Tassi di Reazione di Fusione (Fusion Reaction Rates)}

Il parametro fondamentale che governa la cinetica delle reazioni nucleari di fusione è il tasso di reazione mediato sulla distribuzione di velocità, indicato con $\langle \sigma v \rangle$ e misurato in $[\text{m}^3/\text{s}]$. Dal punto di vista fisico:
\begin{itemize}
    \item Rappresenta la probabilità (per unità di densità e di tempo) che avvenga una collisione efficace di fusione tra due nuclei reagenti.
    \item Dipende fortemente dalla temperatura $T$ del sistema, la quale è direttamente legata all'energia cinetica media delle particelle.
    \item Una volta noti e tabulati, i tassi di reazione $\langle \sigma v \rangle$ velocizzano enormemente il calcolo dei processi di reazione macroscopici.
    \item In linea di principio, la loro valutazione teorica diretta è estremamente complessa poiché richiede:
    \begin{itemize}
        \item Le sezioni d'urto microscopiche $\sigma(v)$ (\emph{cross sections}, determinate sperimentalmente tramite misure fisiche);
        \item La funzione di distribuzione delle velocità dei proiettili (\emph{projectile distribution});
        \item La funzione di distribuzione delle velocità dei bersagli (\emph{target distribution}).
    \end{itemize}
    \item Nella pratica ingegneristica dei reattori a fusione, le distribuzioni di interesse applicativo si discostano raramente dalla distribuzione di equilibrio Maxwelliana (gaussiana nelle componenti di velocità), rendendo possibili e accurati i calcoli analitici e numerici di $\langle \sigma v \rangle$ in funzione della sola temperatura $T$.
\end{itemize}

\begin{figure}[htbp]
\centering
\begin{tikzpicture}
\begin{loglogaxis}[
    width=12cm,
    height=8.5cm,
    xmin=1, xmax=10000,
    ymin=1e-25, ymax=1e-21,
    xlabel={$T\ (\text{keV})$},
    ylabel={$\langle \sigma v \rangle\ (\text{m}^3/\text{s})$},
    grid=both,
    grid style={line width=.1pt, draw=gray!25},
    major grid style={line width=.2pt,draw=gray!50},
    tick align=inside,
    title={\textbf{Fusion Reaction Rates}},
    legend pos=south east
]
    % Fascia azzurra di temperatura ottimale (circa 10-20 keV)
    \fill[cyan!30, opacity=0.6] (axis cs:9,1e-25) rectangle (axis cs:16,1e-21);

    % Curva D-T
    \addplot[thick, black, smooth, line width=1.4pt] coordinates {
        (1.6, 1e-25)
        (2, 2.8e-25)
        (3, 1.8e-24)
        (5, 1.3e-23)
        (10, 1.1e-22)
        (20, 4.3e-22)
        (50, 8.5e-22)
        (70, 8.9e-22)
        (100, 8.4e-22)
        (200, 6.3e-22)
        (500, 3.7e-22)
        (1000, 2.3e-22)
    };
    \addlegendentry{$\text{D}-\text{T}$}

    % Curva D-He3
    \addplot[thick, blue!80!black, dashed, smooth, line width=1.4pt] coordinates {
        (7.5, 1e-25)
        (10, 2.3e-25)
        (20, 3.8e-24)
        (50, 5.4e-23)
        (100, 1.6e-22)
        (200, 2.4e-22)
        (300, 2.5e-22)
        (500, 2.2e-22)
        (1000, 1.5e-22)
    };
    \addlegendentry{$\text{D}-\text{He}^3$}

    % Curva D-D
    \addplot[thick, red!80!black, dashdotted, smooth, line width=1.4pt] coordinates {
        (4.2, 1e-25)
        (10, 1.2e-24)
        (20, 5.2e-24)
        (50, 2.1e-23)
        (100, 4.5e-23)
        (200, 8.8e-23)
        (500, 1.7e-22)
        (1000, 2.2e-22)
    };
    \addlegendentry{$\text{D}-\text{D}$}

    % Annotazioni sul grafico
    \node[anchor=east] at (axis cs:8, 4e-22) {\textbf{D--T}};
    \draw[-{Stealth}, thick] (axis cs:8, 3.5e-22) -- (axis cs:14, 2.2e-22);

    \node[anchor=west] at (axis cs:400, 4e-23) {$\textbf{D--He}^3$};
    \draw[-{Stealth}, thick] (axis cs:450, 5.5e-23) -- (axis cs:220, 2.1e-22);

    \node[anchor=west] at (axis cs:250, 9e-24) {\textbf{D--D}};
    \draw[-{Stealth}, thick] (axis cs:240, 1.2e-23) -- (axis cs:110, 4.5e-23);
\end{loglogaxis}
\node[below=0.2cm, text width=11cm, align=center, font=\small\itshape] at (6, -0.8) {
    La banda verticale azzurra ($T \approx 10\text{ keV}$) indica il limite inferiore pratico di temperatura per avere un'alta probabilit\`a di reazione e un buon valore di $\langle \sigma v \rangle$.
};
\end{tikzpicture}
\caption{Andamento del tasso di reazione $\langle \sigma v \rangle$ in funzione della temperatura $T\ (\text{keV})$ per le principali reazioni di fusione ($\text{D}-\text{T}$, $\text{D}-\text{He}^3$, $\text{D}-\text{D}$).}
\label{fig:reaction_rates}
\end{figure}

\subsection{Unità di Misura Tipiche per un Plasma Termonucleare}

Nella fisica dei plasmi a fusione, la temperatura $T$ viene comunemente espressa in unità di energia, ovvero in elettronvolt ($\text{eV}$) o kiloelettronvolt ($\text{keV}$). Per comprendere il significato fisico di una temperatura minima richiesta dell'ordine di $2\text{ keV}$ (o $10\text{ keV}$ per l'ignizione), richiamiamo la definizione di elettronvolt:
\begin{equation}
    1\text{ eV} = 1.602 \cdot 10^{-19}\text{ J}
\end{equation}
Questa quantità corrisponde a:
\begin{itemize}
    \item L'energia cinetica acquisita da un elettrone ($e^-$) accelerato attraverso una differenza di potenziale $\Delta V = 1\text{ V}$;
    \item Il lavoro compiuto da una forza elettrostatica per spostare un elettrone ($e^-$) attraverso una barriera di potenziale di $1\text{ V}$.
\end{itemize}

\begin{figure}[htbp]
\centering
\begin{tikzpicture}[scale=0.95]
    % --- Schema Sinistro: Definizione di 1 eV ---
    \begin{scope}[shift={(0,0)}]
        % Griglia sinistra (-)
        \draw[thick] (0,0) -- (1.2,0.8) -- (1.2,3.8) -- (0,3.0) -- cycle;
        \foreach \y in {0.5, 1.0, 1.5, 2.0, 2.5} {
            \draw[thin] (0,\y) -- (1.2,\y+0.8);
        }
        \foreach \x in {0.3, 0.6, 0.9} {
            \draw[thin] (\x, \x*0.66) -- (\x, \x*0.66+3.0);
        }
        \foreach \y in {0.6, 1.3, 2.0, 2.7, 3.4} {
            \node[font=\bfseries\large] at (1.6, \y) {$-$};
        }

        % Elettrone e freccia di accelerazione
        \filldraw[fill=red!20, draw=red!80!black, thick] (0.8, 1.9) circle (0.3cm);
        \node[font=\bfseries] at (0.8, 1.9) {$-$};
        \draw[-{Stealth[length=3mm]}, thick] (1.4, 1.9) -- (3.2, 1.9);

        % Griglia destra (+)
        \draw[thick] (5.0,0) -- (6.2,0.8) -- (6.2,3.8) -- (5.0,3.0) -- cycle;
        \foreach \y in {0.5, 1.0, 1.5, 2.0, 2.5} {
            \draw[thin] (5.0,\y) -- (6.2,\y+0.8);
        }
        \foreach \x in {5.3, 5.6, 5.9} {
            \draw[thin] (\x, {(\x-5.0)*0.66}) -- (\x, {(\x-5.0)*0.66+3.0});
        }
        \foreach \y in {0.6, 1.3, 2.0, 2.7, 3.4} {
            \node[font=\bfseries\large] at (6.6, \y) {$+$};
        }

        % Batteria 1 Volt
        \draw[fill=green!40!gray!60, draw=black, rounded corners=4pt] (1.8,-1.8) rectangle (4.2,-0.7);
        \draw[fill=green!60!black, draw=black, rounded corners=2pt] (4.2,-1.5) rectangle (4.5,-1.0);
        \node[font=\bfseries] at (3.0,-1.25) {$1\text{ Volt}$};

        % Fili di collegamento
        \draw[thick] (0.5, 0.2) .. controls (1.2, -0.5) and (0.5, -1.4) .. (1.8, -1.25);
        \draw[thick] (5.6, 0.4) .. controls (6.3, -0.4) and (5.0, -1.1) .. (4.5, -1.25);
    \end{scope}

    % --- Schema Destro: Distribuzione Gaussiana delle particelle ---
    \begin{scope}[shift={(8.2,0.2)}]
        \draw[-{Stealth[length=3mm]}, thick] (0,0) -- (6.0,0) node[below left] {$E, v$};
        \draw[thick, line width=1.2pt, domain=0.3:5.5, samples=100] plot (\x, {2.8*exp(-(\x-2.9)^2/1.6)});
        % Punti che rappresentano le particelle sotto la curva
        \foreach \px/\py in {1.8/0.6, 2.2/1.1, 2.5/0.8, 2.8/1.9, 3.0/1.3, 3.2/2.3, 3.5/1.2, 3.8/0.7, 4.1/1.0, 2.9/0.4} {
            \fill (\px,\py) circle (1.2pt);
        }
        \draw[-{Stealth}, thick] (4.0, 2.1) .. controls (4.5, 2.4) and (5.0, 2.4) .. (5.5, 2.3);
        \node[right, text width=3.2cm, font=\small\itshape] at (5.5, 2.0) {gas particles describe a Gaussian distribution};
    \end{scope}
\end{tikzpicture}
\caption{A sinistra: interpretazione fisica dell'elettronvolt ($1\text{ eV}$) tramite accelerazione elettrostatica di un elettrone $e^-$ attraverso $\Delta V = 1\text{ V}$. A destra: distribuzione gaussiana (maxwelliana) dell'energia delle particelle nel gas.}
\label{fig:ev_and_gaussian}
\end{figure}

Per mettere in relazione l'energia termica espressa in $\text{eV}$ con la temperatura termodinamica assoluta $T$ espressa in Kelvin ($\text{K}$), si utilizza la costante di Boltzmann $k_B$:
\begin{equation}
    E_{th} = k_B T \quad \text{con} \quad k_B = 1.38 \cdot 10^{-23}\ \frac{\text{J}}{\text{K}}
\end{equation}
Ponendo $E_{th} = 1\text{ eV}$, si ricava la temperatura equivalente in Kelvin:
\begin{equation}
    T\ [\text{K}] = \frac{E_{th}\ [\text{J}]}{k_B} = \frac{1.602 \cdot 10^{-19}\text{ J}}{1.38 \cdot 10^{-23}\text{ J/K}} \simeq 11600\text{ K} = 1.16 \cdot 10^{4}\text{ K}
\end{equation}
Di conseguenza, una temperatura di $2\text{ keV}$ equivale a:
\begin{equation}
    2\text{ keV} \sim 2 \cdot 10^{7}\text{ K}
\end{equation}
mentre la temperatura minima operativa di un plasma di fusione, pari a circa $10\text{ keV}$, corrisponde a:
\begin{equation}
    T \sim 10\text{ keV} \longrightarrow 100 \cdot 10^{6}\ ^\circ\text{C} \quad (\text{circa } 10^8\text{ K})
\end{equation}

\pagebreak

\subsection{Il Quarto Stato della Materia: Il Plasma}
Dall'analisi della Figura~\ref{fig:reaction_rates} emergono tre considerazioni fondamentali:
\begin{enumerate}
    \item La reazione di fusione Deuterio-Trizio ($\text{D}-\text{T}$) presenta \textbf{sempre la più alta probabilità} (ovvero il massimo valore di $\langle \sigma v \rangle$) a qualsiasi temperatura assegnata di interesse pratico.
    \item La temperatura minima necessaria affinché la sezione d'urto maxwelliana sia sufficientemente elevata è di circa $10\text{ keV}$ ($\sim 100 \cdot 10^6\ ^\circ\text{C}$).
    \item A temperature così estreme, la materia non può esistere allo stato solido, liquido o gassoso ordinario, ma si trova sotto forma di \textbf{PLASMA} (\emph{4th state of matter}), ossia un gas completamente ionizzato.
\end{enumerate}

L'obiettivo principale di questa trattazione è caratterizzare il plasma di fusione nucleare rispondendo a due quesiti chiave:
\begin{enumerate}
    \item Come ``generare'' un plasma (\emph{how to generate a plasma});
    \item Come mantenerlo ``acceso'' (\emph{how to maintain it switched on / ignite it}).
\end{enumerate}

% ============================================================================
\section{Generazione del Plasma e Reazione di Fusione D--T}
% ============================================================================

\subsection{Ionizzazione di un Gas in un Volume di Controllo}
Per generare un plasma si riempie una ``scatola'' (volume di confinamento) con un gas neutro e lo si ionizza fornendo un'energia in ingresso $E_{in}$ superiore o uguale all'energia di legame/ionizzazione $E_{ion}$ ($E_{in} \ge E_{ion}$). In questo modo si rompe il legame coulombiano tra gli elettroni ($e^-$) e il nucleo (composto da protoni $P$ e neutroni $N$), espellendo gli elettroni e ottenendo un insieme di ioni positivi e di elettroni liberi.

\begin{figure}[htbp]
\centering
\begin{tikzpicture}[scale=0.95]
    % Atomo di Deuterio a sinistra
    \draw[dashed, thick, gray] (0,0) circle (1.2cm);
    \filldraw[fill=orange!80, draw=black] (-0.15,0) circle (0.25cm) node[font=\scriptsize\bfseries] {N};
    \filldraw[fill=red!70, draw=black] (0.18,0) circle (0.25cm) node[font=\scriptsize\bfseries, white] {P};
    \filldraw[fill=cyan!60, draw=black] (0.85,0.85) circle (0.18cm) node[font=\scriptsize] {$e^-$};
    \node[above=0.1cm] at (0,1.3) {\textbf{Deuterium}};

    % Freccia di iniezione di energia E_in >= E_ion
    \draw[-{Stealth[length=3mm]}, very thick, red!80!black] (-1.8, -0.8) -- (-0.5, -0.2);
    \node[below left, text width=4.2cm, align=center, font=\small] at (-1.2, -0.8) {
        $E_{in} \ge E_{ion}$\\
        \scriptsize ($E_{in}$ injected to break the bond between $e^-$ and nucleus)
    };

    % Freccia verso il box di plasma
    \draw[-{Stealth[length=3mm]}, very thick] (1.6, 0) -- (3.4, 0) node[midway, below=0.1cm] {$\Delta T$};

    % Box con il gas ionizzato (plasma)
    \draw[very thick] (3.8, -1.8) rectangle (8.5, 1.8);
    \node[above] at (6.15, 1.8) {\textbf{Ionized Gas in a ``Box''}};

    % Ioni positivi e elettroni liberi dentro il box
    \filldraw[fill=red!30, draw=black] (4.8, 0.9) circle (0.28cm) node[font=\scriptsize] {NP$^+$};
    \filldraw[fill=red!30, draw=black] (7.2, 0.7) circle (0.28cm) node[font=\scriptsize] {$+$};
    \filldraw[fill=red!30, draw=black] (5.5, -0.8) circle (0.28cm) node[font=\scriptsize] {$+$};
    \filldraw[fill=red!30, draw=black] (7.5, -0.9) circle (0.28cm) node[font=\scriptsize] {$+$};

    \node[font=\bfseries, blue!80!black] at (4.5, -0.3) {$e^-$};
    \node[font=\bfseries, blue!80!black] at (6.2, 1.1) {$e^-$};
    \node[font=\bfseries, blue!80!black] at (6.4, -0.1) {$e^-$};
    \node[font=\bfseries, blue!80!black] at (4.6, -1.3) {$e^-$};
    \node[font=\bfseries, blue!80!black] at (7.8, 0.0) {$e^-$};

    \draw[-{Stealth}, thick] (9.5, 1.2) -- (5.1, 1.0);
    \node[right, font=\small] at (9.5, 1.2) {Positive ions of D--T};
\end{tikzpicture}
\caption{Processo di ionizzazione di un atomo di deuterio mediante somministrazione di energia $E_{in} \ge E_{ion}$ e formazione del plasma all'interno di un volume di confinamento.}
\label{fig:ionization_box}
\end{figure}

\pagebreak

\subsection{Scelta della Miscela Gassosa e Reazione D--T}
Quale gas conviene utilizzare per alimentare un reattore a fusione? Confrontando le tre reazioni principali ($\text{D}-\text{T}$, $\text{D}-\text{He}^3$, $\text{D}-\text{D}$):
\begin{itemize}
    \item I prodotti di reazione sono differenti per ciascun ciclo;
    \item La reazione $\text{D}-\text{T}$ produce la più alta energia per singola reazione (escludendo la reazione $\text{D}-\text{He}^3$, la quale tuttavia richiede temperature molto più elevate e utilizza l'isotopo $\text{He}^3$, estremamente raro sulla Terra e presente in quantità significative solo sulla superficie lunare);
    \item La reazione $\text{D}-\text{T}$ è la più agevole dal punto di vista termodinamico (\emph{easier temperature-wise}), poiché raggiunge il picco di reattività alla temperatura più bassa.
\end{itemize}

La reazione di fusione tra un nucleo di Deuterio ($^2\text{H}$ o $\text{D}$, costituito da 1 protone e 1 neutrone) e un nucleo di Trizio ($^3\text{H}$ o $\text{T}$, costituito da 1 protone e 2 neutroni) genera una particella $\alpha$ ($^4\text{He}$, nucleo di elio composto da 2 protoni e 2 neutroni) e un neutrone libero ($n$):
\begin{equation}
    \text{D} + \text{T} \longrightarrow n\ (14.1\text{ MeV}) + \alpha\ (3.5\text{ MeV})
\end{equation}

\begin{figure}[htbp]
\centering
\begin{tikzpicture}[scale=0.95]
    % Deuterio (2H)
    \filldraw[fill=cyan!60, draw=blue!80!black, thick] (-4.2, 1.6) circle (0.35cm);
    \filldraw[fill=cyan!40, draw=blue!80!black, thick] (-3.8, 1.4) circle (0.35cm);
    \node[below] at (-4.0, 0.95) {\textbf{$^2\text{H}$ (D)}};

    % Trizio (3H)
    \filldraw[fill=cyan!60, draw=blue!80!black, thick] (-4.3, -1.4) circle (0.35cm);
    \filldraw[fill=cyan!40, draw=blue!80!black, thick] (-3.8, -1.2) circle (0.35cm);
    \filldraw[fill=cyan!50, draw=blue!80!black, thick] (-3.9, -1.7) circle (0.35cm);
    \node[above] at (-4.0, -0.8) {\textbf{$^3\text{H}$ (T)}};

    % Frecce verso il centro di reazione
    \draw[-{Stealth[length=3mm]}, very thick, gray!80!black] (-3.1, 1.3) -- (-1.2, 0.4);
    \draw[-{Stealth[length=3mm]}, very thick, gray!80!black] (-3.1, -1.3) -- (-1.2, -0.4);

    % Stella centrale (collisione di fusione)
    \node[star, star points=10, star point ratio=2.2, fill=orange!85!yellow, draw=red!80!black, thick, minimum size=2.2cm] at (0,0) {};
    \node[circle, fill=white, minimum size=0.5cm] at (0,0) {};
    \node[above=1.2cm, font=\bfseries\small] at (0,0) {FUSION REACTION};

    % Frecce verso i prodotti
    \draw[-{Stealth[length=3mm]}, very thick, gray!80!black] (1.2, 0.4) -- (3.1, 1.3);
    \draw[-{Stealth[length=3mm]}, very thick, gray!80!black] (1.2, -0.4) -- (3.1, -1.3);

    % Neutrone (n + 14.1 MeV)
    \filldraw[fill=cyan!60, draw=blue!80!black, thick] (4.0, 1.8) circle (0.35cm);
    \node[below] at (4.2, 1.35) {\textbf{$n + 14.1\text{ MeV}$}};

    % Elio-4 (4He + 3.5 MeV) cerchiato in rosso
    \begin{scope}[shift={(4.2, -1.7)}]
        \filldraw[fill=cyan!60, draw=blue!80!black, thick] (-0.25, 0.2) circle (0.35cm);
        \filldraw[fill=cyan!40, draw=blue!80!black, thick] (0.25, 0.2) circle (0.35cm);
        \filldraw[fill=cyan!50, draw=blue!80!black, thick] (-0.3, -0.25) circle (0.35cm);
        \filldraw[fill=cyan!30, draw=blue!80!black, thick] (0.2, -0.25) circle (0.35cm);
        \node[above] at (0, 0.65) {\textbf{$^4\text{He} + 3.5\text{ MeV}$}};
        \draw[very thick, red!80!black] (0, 0.1) circle (1.45cm);
    \end{scope}

    % Freccia rossa di auto-riscaldamento
    \draw[-{Stealth[length=3mm]}, very thick, red!80!black] (4.2, -3.15) -- (0.2, -3.9);
    \node[below, font=\bfseries, red!80!black] at (0, -3.9) {Potentially able to self-heat the plasma!};
\end{tikzpicture}
\caption{Schema della reazione di fusione $\text{D}+\text{T} \rightarrow n + \alpha$. La particella carica $^4\text{He}$ da $3.5\text{ MeV}$ rimane confinata ed è potenzialmente in grado di auto-riscaldare il plasma.}
\label{fig:dt_fusion_reaction}
\end{figure}

\subsubsection{Bilancio di Energia a Riposo}
Considerando l'energia a riposo associata alle masse e ai legami nucleari dei reagenti e dei prodotti:
\begin{align}
    \text{Energia a riposo dei reagenti:} \quad & E_D + E_T \simeq 4685\text{ MeV} \\
    \text{Energia a riposo dei prodotti:} \quad & E_n + E_{\text{He}^4} \simeq 4667\text{ MeV}
\end{align}
La variazione di energia a riposo $\Delta E$ liberata nella reazione sotto forma di energia cinetica dei prodotti ($E_{fus}$) risulta pari a:
\begin{equation}
    \Delta E = E_{fus} = 4685 - 4667 \simeq 17.6\text{ MeV}
\end{equation}

\subsection{Ripartizione dell'Energia tra i Prodotti di Reazione}
Perché l'energia liberata $E_{fus} = 17.6\text{ MeV}$ si ripartisce esattamente in $14.1\text{ MeV}$ sul neutrone e $3.5\text{ MeV}$ sul nucleo di $^4\text{He}$? La suddivisione deriva dai principi di \textbf{conservazione dell'energia cinetica e della quantità di moto}. Assumendo che il sistema $\text{D}+\text{T}$ abbia quantità di moto totale approssimativamente nulla nel sistema del centro di massa prima dell'urto (energia cinetica iniziale di pochi $\text{keV}$ trascurabile rispetto a $17.6\text{ MeV}$), imponiamo il sistema di conservazione:
\begin{equation}
\begin{cases}
    \dfrac{1}{2} m_n v_n^2 + \dfrac{1}{2} m_{\text{He}} v_{\text{He}}^2 = E_{fus} = 17.6\text{ MeV} \\[0.3cm]
    m_n v_n + m_{\text{He}} v_{\text{He}} = 0
\end{cases}
\end{equation}
Dalla seconda equazione (conservazione della quantità di moto) si ricava la velocità del nucleo di elio in funzione di quella del neutrone:
\begin{equation}
    v_{\text{He}} = - v_n \frac{m_n}{m_{\text{He}}}
\end{equation}
Sostituendo tale espressione nella prima equazione (conservazione dell'energia cinetica):
\begin{align}
    \frac{1}{2} m_n v_n^2 + \frac{1}{2} m_{\text{He}} \left( - v_n \frac{m_n}{m_{\text{He}}} \right)^2 &= E_{fus} \nonumber \\
    \frac{1}{2} v_n^2 \left( m_n + \frac{m_n^2}{m_{\text{He}}} \right) &= E_{fus} \nonumber \\
    \frac{1}{2} m_n v_n^2 \left( 1 + \frac{m_n}{m_{\text{He}}} \right) &= E_{fus}
\end{align}
Ricordando che l'energia cinetica del neutrone è $E_{k,n} = \frac{1}{2} m_n v_n^2$, si ottiene:
\begin{equation}
    E_{k,n} = \frac{1}{2} m_n v_n^2 = E_{fus} \left( \frac{m_{\text{He}}}{m_{\text{He}} + m_n} \right)
\end{equation}
In modo del tutto simmetrico, l'energia cinetica della particella $\alpha$ ($^4\text{He}$) vale:
\begin{equation}
    E_{k,\text{He}} = \frac{1}{2} m_{\text{He}} v_{\text{He}}^2 = E_{fus} \left( \frac{m_n}{m_{\text{He}} + m_n} \right)
\end{equation}
Calcolando il rapporto tra le energie cinetiche dei due prodotti e semplificando il denominatore comune:
\begin{equation}
    \frac{E_{k,n}}{E_{k,\text{He}}} = \frac{\dfrac{m_{\text{He}}}{\cancel{m_{\text{He}} + m_n}}}{\dfrac{m_n}{\cancel{m_{\text{He}} + m_n}}} = \frac{m_{\text{He}}}{m_n} \simeq \frac{4\text{ u.m.a.}}{1\text{ u.m.a.}} = 4
\end{equation}
Mettendo a sistema il rapporto delle energie con l'energia totale di fusione $E_{fus} = 17.6\text{ MeV}$:
\begin{equation}
\begin{cases}
    \dfrac{E_{k,n}}{E_{k,\text{He}}} = 4 \implies E_n = 4 E_{\text{He}} \\[0.3cm]
    E_{k,n} + E_{k,\text{He}} = E_{fus} = 17.6\text{ MeV}
\end{cases}
\end{equation}
Si risolve per sostituzione diretta:
\begin{align}
    4 E_{\text{He}} + E_{\text{He}} = 17.6\text{ MeV} &\implies E_{\text{He}} = \frac{17.6}{5}\text{ MeV} = \frac{1}{5} E_{fus} \simeq 3.5\text{ MeV} \\
    E_n = 17.6 - E_{\text{He}} &\implies E_n = \frac{4}{5} E_{fus} \simeq 14.1\text{ MeV}
\end{align}
La particella più leggera (il neutrone $n$) acquisisce i $4/5$ dell'energia di fusione, mentre il nucleo di $^4\text{He}$, essendo elettricamente carico, rimane confinato all'interno del campo magnetico e cede i suoi $3.5\text{ MeV}$ per riscaldare il plasma (\emph{self-heating}).

\subsection{Generazione di un Plasma D--T 50/50 e Quasi-Neutralità}
Consideriamo una miscela stechiometrica bilanciata composta al $50\%$ da Deuterio ($\text{D}$) e al $50\%$ da Trizio ($\text{T}$). Riscaldando il gas ad alta temperatura ($T \sim 10\text{ keV}$), si raggiunge la ionizzazione completa:
\begin{align}
    \text{D} &\longrightarrow \text{D}^+ + e^- \\
    \text{T} &\longrightarrow \text{T}^+ + e^-
\end{align}

\begin{figure}[htbp]
\centering
\begin{tikzpicture}[scale=0.95]
    % Box Sinistro: Gas neutro 50% D e 50% T
    \draw[very thick] (0,0) rectangle (3.6, 3.2);
    \node[above] at (1.8, 3.2) {\textbf{Neutral Gas ($50\%\text{ D}, 50\%\text{ T}$)}};
    \node[font=\large\bfseries] at (0.8, 2.4) {D};
    \node[font=\large\bfseries] at (2.6, 2.5) {T};
    \node[font=\large\bfseries] at (1.8, 1.6) {D};
    \node[font=\large\bfseries] at (0.9, 0.8) {T};
    \node[font=\large\bfseries] at (2.8, 1.2) {D};
    \node[font=\large\bfseries] at (2.1, 0.6) {T};

    % Freccia di ionizzazione ad alta T
    \draw[-{Stealth[length=3mm]}, very thick] (4.0, 1.6) -- (6.6, 1.6);
    \node[above, align=center, font=\small\bfseries] at (5.3, 1.65) {@ high $T$ ($10\text{ keV}$)\\ionization};

    % Box Destro: Plasma ionizzato
    \draw[very thick] (7.0,0) rectangle (11.2, 3.2);
    \node[above] at (9.1, 3.2) {\textbf{DT Plasma}};
    \node[font=\bfseries, red!80!black] at (7.8, 2.6) {$\text{D}^+$};
    \node[font=\bfseries, red!80!black] at (9.2, 2.7) {$\text{T}^+$};
    \node[font=\bfseries, red!80!black] at (10.5, 2.3) {$\text{D}^+$};
    \node[font=\bfseries, red!80!black] at (8.1, 1.4) {$\text{T}^+$};
    \node[font=\bfseries, red!80!black] at (9.8, 1.5) {$\text{D}^+$};
    \node[font=\bfseries, red!80!black] at (8.9, 0.5) {$\text{T}^+$};

    \node[font=\bfseries, blue!80!black] at (8.5, 2.2) {$e^-$};
    \node[font=\bfseries, blue!80!black] at (7.5, 1.9) {$e^-$};
    \node[font=\bfseries, blue!80!black] at (10.3, 1.0) {$e^-$};
    \node[font=\bfseries, blue!80!black] at (7.7, 0.6) {$e^-$};
    \node[font=\bfseries, blue!80!black] at (9.4, 1.0) {$e^-$};
    \node[font=\bfseries, blue!80!black] at (10.6, 2.8) {$e^-$};
\end{tikzpicture}
\caption{Ionizzazione completa di una miscela al $50\%$ di Deuterio e $50\%$ di Trizio ad alta temperatura ($10\text{ keV}$).}
\label{fig:dt_mixture_ionization}
\end{figure}

Definendo la densità numerica $n$ come il numero di particelle per unità di volume $\left[\frac{\#}{\text{m}^3}\right]$, nell'esempio schematico di Figura~\ref{fig:dt_mixture_ionization} (con volume unitario) si contano:
\begin{equation}
    n_T = 3\ \left[\frac{\#}{\text{m}^3}\right], \quad n_D = 3\ \left[\frac{\#}{\text{m}^3}\right] \implies n_e = 6\ \left[\frac{\#}{\text{m}^3}\right]
\end{equation}
In generale, per un plasma puro ed elettricamente neutro (\emph{quasi-neutrality}) generato da una miscela equimolare $50/50$ di Deuterio e Trizio, vale la relazione fondamentale:
\begin{equation}
    n_D = n_T = \frac{n_e}{2} \iff n_e = n_D + n_T = 2 n_D
\end{equation}

\pagebreak

\subsection{Confinamento del Plasma: Il Tokamak}
Qual è la ``scatola'' fisica in cui viene confinato il plasma termonucleare? Poiché nessun materiale solido può resistere al contatto diretto con un gas a $10^8\text{ K}$ senza raffreddarlo istantaneamente e subire danni strutturali, il confinamento avviene per via magnetica all'interno di un "Tokamak", e più precisamente all'interno della sua camera da vuoto toroidale (\emph{vacuum vessel}), che viene inizialmente riempita con il gas neutro.

\begin{figure}[htbp]
\centering
\begin{tikzpicture}[scale=0.95]
    % Vista schematica del Tokamak (Solenoide centrale + Sezione Poloidale a D + Direzione Toroidale)
    % Solenoide Centrale
    \draw[fill=gray!30, draw=black, thick] (-0.8, -2.8) rectangle (0.8, 2.8);
    \foreach \y in {-2.4, -1.8, -1.2, -0.6, 0.0, 0.6, 1.2, 1.8, 2.4} {
        \draw[thick, gray!70!black] (-0.8, \y) -- (0.8, \y);
    }
    \node[rotate=90, font=\small\bfseries] at (0,0) {CENTRAL SOLENOID};

    % Camera da vuoto sinistra (in sezione)
    \draw[very thick, gray!80!black, fill=orange!10] (-1.2, -2.0) -- (-1.2, 2.0) .. controls (-3.8, 2.0) and (-4.2, -1.2) .. (-2.2, -2.5) -- cycle;

    % Camera da vuoto destra con Sezione Poloidale evidenziata in azzurro (come a Pagina 19)
    \draw[very thick, gray!80!black, fill=red!10] (1.2, -2.0) -- (1.2, 2.0) .. controls (3.8, 2.0) and (4.2, -1.2) .. (2.2, -2.5) -- cycle;

    % Contorno azzurro della sezione poloidale del plasma con punto X (divertore) in basso
    \draw[cyan!80!blue, line width=2.2pt] (1.5, 0.2) .. controls (1.5, 1.8) and (3.2, 1.7) .. (3.3, 0.1) .. controls (3.4, -1.0) and (2.6, -1.7) .. (2.1, -2.3) -- (2.5, -2.6);
    \draw[cyan!80!blue, line width=2.2pt] (1.5, 0.2) .. controls (1.5, -1.0) and (1.9, -1.7) .. (2.5, -2.3) -- (1.9, -2.6);

    % Annotazioni
    \node[above, font=\bfseries, cyan!60!blue] at (2.6, 2.1) {POLOIDAL SECTION};
    \draw[-{Stealth}, thick, cyan!60!blue] (3.6, 1.2) -- (4.8, 1.5) node[right, text width=3.5cm, font=\small, black] {Typical D-shape of the plasma section (with lower X-point)};

    % Freccia Direzione Toroidale
    \draw[-{Stealth[length=3mm]}, very thick, red!80!black] (-2.8, -0.3) arc (200:340:2.8cm and 0.9cm);
    \node[below, font=\bfseries, red!80!black] at (0, -3.1) {TOROIDAL DIRECTION ($\vec{B}_{tor}, \vec{J}$)};
\end{tikzpicture}
\caption{Rappresentazione geometrica del Tokamak: solenoide centrale, camera da vuoto (\emph{vacuum vessel}), direzione toroidale e sezione poloidale a forma di ``D'' con punto X inferiore.}
\label{fig:tokamak_geometry}
\end{figure}

le particelle cariche devono rimanere confinate nel volume magnetico toroidale per un tempo sufficientemente lungo: se vi rimangono per troppo poco tempo, il numero di collisioni efficaci è insufficiente a sostenere la reazione.

% ============================================================================
\section{Sostenibilità del Plasma e Tempo di Confinamento ($\tau_E$)}
% ============================================================================

\subsection{Mantenimento di un ``Burning Plasma''}
Una volta generato il plasma ionizzato di atomi $\text{D}-\text{T}$, l'obiettivo di un reattore è produrre energia netta: è quindi indispensabile che le reazioni di fusione continuino ad avvenire per un tempo sufficientemente lungo (\emph{burning plasma}).

\begin{figure}[htbp]
\centering
\begin{tikzpicture}[scale=0.95]
    % Box del Burning Plasma
    \draw[very thick, fill=orange!5] (0,0) rectangle (6.5, 4.0);
    \node[above] at (3.25, 4.0) {\textbf{Fully-Ionized Gas = Burning Plasma}};

    % Particelle del plasma
    \node[font=\bfseries, red!80!black] at (1.0, 3.3) {$\text{D}^+$};
    \node[font=\bfseries, red!80!black] at (2.4, 3.4) {$\text{T}^+$};
    \node[font=\bfseries, red!80!black] at (5.2, 3.2) {$\text{D}^+$};
    \node[font=\bfseries, red!80!black] at (1.2, 1.0) {$\text{T}^+$};
    \node[font=\bfseries, red!80!black] at (5.5, 1.2) {$\text{T}^+$};

    \node[font=\bfseries, blue!80!black] at (1.7, 2.8) {$e^-$};
    \node[font=\bfseries, blue!80!black] at (4.3, 3.4) {$e^-$};
    \node[font=\bfseries, blue!80!black] at (0.8, 2.0) {$e^-$};
    \node[font=\bfseries, blue!80!black] at (5.8, 2.3) {$e^-$};
    \node[font=\bfseries, blue!80!black] at (2.2, 0.7) {$e^-$};

    % Evento di reazione al centro
    \draw[thick, red, fill=yellow!40] (3.2, 2.0) circle (0.45cm);
    \node[font=\scriptsize\bfseries] at (3.2, 2.0) {$\text{D}^+\!+\!\text{T}^+$};

    % He-4 rimane nel plasma
    \draw[-{Stealth[length=2.5mm]}, very thick, green!50!black] (3.0, 1.6) .. controls (2.5, 1.0) and (3.5, 0.6) .. (4.2, 1.2);
    \node[below, font=\small\bfseries, green!40!black] at (3.8, 0.7) {$\text{He}^4 \rightarrow 3.5\text{ MeV}$};
    \node[below, font=\scriptsize\itshape, green!40!black] at (3.8, 0.3) {(remains in plasma $\rightarrow$ self-heating)};

    % Neutrone esce dal plasma
    \draw[-{Stealth[length=3mm]}, very thick, purple!80!black] (3.65, 2.0) -- (8.2, 2.0);
    \filldraw[fill=purple!70, draw=black] (8.5, 2.0) circle (0.18cm) node[above=0.15cm, font=\small\bfseries] {$n\ (14.1\text{ MeV})$};
    \node[below right, text width=4.8cm, font=\scriptsize] at (6.7, 1.7) {
        Not confined because uncharged! Goes to the wall / Balance of Plant (BoP).
    };
\end{tikzpicture}
\caption{Comportamento dei prodotti di fusione in un \emph{burning plasma}: la particella carica $\text{He}^4$ ($3.5\text{ MeV}$) resta confinata e riscalda il plasma, mentre il neutrone $n$ ($14.1\text{ MeV}$) fugge verso la parete.}
\label{fig:burning_plasma}
\end{figure}

\pagebreak

Analizziamo il bilancio fisico del sistema mostrato in Figura~\ref{fig:burning_plasma}:
\begin{itemize}
    \item Naturalmente il plasma tende a ``spegnersi'' (\emph{switch off}, nel senso che la sua temperatura $T$ tende a diminuire) e a perdere energia verso l'esterno.
    \item I \textbf{neutroni} ($14.1\text{ MeV}$), essendo privi di carica elettrica, non risentono del confinamento magnetico e vengono ``persi'' dal plasma, ma \textbf{non sono persi per il Balance of Plant (BoP)}, il quale ha proprio la funzione ingegneristica di catturare i neutroni nel \emph{blanket} ed estrarre l'energia termica da essi depositata per produrre potenza elettrica.
    \item Se le perdite interne sono inferiori o uguali all'energia guadagnata tramite il riscaldamento delle particelle $\alpha$ ($3.5\text{ MeV}$), è possibile sostenere il plasma: l'affermazione che sia teoricamente impossibile sostenerlo è dunque \textbf{falsa} ($\cancel{\text{True}} \rightarrow \text{False}$).
\end{itemize}

\subsection{Definizione Fisica del Tempo di Confinamento ($\tau_E$)}
La condizione di stazionarietà per sostenere un plasma senza riscaldamento esterno aggiuntivo (condizione di ignizione) richiede che la densità di potenza interna prodotta dalle fusioni e rilasciata nel plasma ($W_{int}$) eguagli la densità di potenza persa dal sistema ($W_{out}$):
\begin{equation}
    W_{int} = W_{out} \quad \left[\frac{\text{energia}}{\text{s}\cdot\text{m}^3} = \frac{\text{W}}{\text{m}^3}\right]
\end{equation}
Dove la densità di potenza interna depositata dalle particelle $\alpha$ ($\text{He}^4$) è data da:
\begin{equation}
    W_{int} = E_{\text{He}}\, n_D n_T \langle \sigma v \rangle_{DT} \quad (\text{con } E_{\text{He}} = E_\alpha = 3.5\text{ MeV})
\end{equation}
Per modellare le perdite energetiche complessive del plasma, si osserva che la potenza persa per unità di volume $W_{out}$ è proporzionale alla densità di energia interna del plasma $E_{int}$ tramite una costante di proporzionalità che racchiude tutti i meccanismi di trasporto e perdita, detta \textbf{tempo di confinamento dell'energia} $\tau_E$:
\begin{equation}
    W_{out} \propto E_{int} \implies W_{out} = \frac{E_{int}}{\tau_E} = -\frac{dE_{int}}{dt}
\end{equation}
Da cui si definisce formalmente il tempo di confinamento dell'energia $\tau_E$:
\begin{equation}
    \tau_E = \frac{E_{int}}{W_{out}}
\end{equation}
Esso rappresenta il tempo caratteristico in cui l'energia interna del plasma viene persa in assenza di sorgenti di riscaldamento. Infatti, risolvendo per separazione delle variabili l'equazione differenziale ordinaria del decadimento libero dell'energia interna:
\begin{equation}
    \frac{dE_{int}}{dt} = -\frac{E_{int}}{\tau_E} \implies \int_{E_{int}(t_0)}^{E_{int}(t)} \frac{dE_{int}}{E_{int}} = -\int_{t_0}^{t} \frac{dt}{\tau_E}
\end{equation}
Integrando ambo i membri e ponendo l'istante iniziale $t_0 = 0$:
\begin{equation}
    \Big[ \ln(E_{int}) \Big]_{E_{int}(t_0)}^{E_{int}(t)} = -\frac{t - \cancelto{0}{t_0}}{\tau_E} \implies \ln\left(\frac{E_{int}(t)}{E_{int}(t_0)}\right) = -\frac{t}{\tau_E}
\end{equation}
Passando all'esponenziale si ottiene la legge di decadimento esponenziale dell'energia interna:
\begin{equation}
    E_{int}(t) = E_{int}(t_0)\, e^{-t/\tau_E}
\end{equation}

\subsection{Pressione del Plasma e Approcci al Confinamento}
Per un plasma ideale completamente ionizzato costituito da elettroni (densità $n_e$) e ioni (densità $n_i = n_D + n_T = n_e$) alla medesima temperatura $T$ (espressa in unità energetiche, incorporando $k_B$), la pressione totale $p$ è data dalla somma delle pressioni parziali:
\begin{equation}
    p = (n_e + n_i) T = 2 n_e T
\end{equation}
Oltre a raggiungere temperature molto elevate ($T \gg$), per ottenere l'ignizione (\emph{Ignite!}) è necessario raggiungere un'adeguata pressione $p$ e mantenere l'energia interna $E_{int}$ confinata per un tempo $\tau_E$ sufficientemente lungo. I due principali sistemi di confinamento sfruttano principi opposti per massimizzare il prodotto $n \tau_E$:
\begin{enumerate}
    \item \textbf{Confinamento Magnetico (MCF, es. Tokamak)}: punta ad avere $\tau_E \gg$ (tempi di confinamento dell'ordine dei secondi) operando a densità $n$ relativamente basse ($\sim 10^{20}\text{ m}^{-3}$).
    \item \textbf{Confinamento Inerziale (ICF)}: punta ad avere $n \gg$ (densità estreme per compressione) su tempi $\tau_E$ brevissimi.
\end{enumerate}

% ============================================================================
\section{Criterio di Lawson: Ignizione Non Ideale ed Effetto delle Impurezze}
% ============================================================================

\subsection{Inquinamento del Plasma e Diluizione del Combustibile}
In un reattore reale, il plasma non è composto esclusivamente da ioni di combustibile $\text{D}^+$, $\text{T}^+$ ed elettroni corrispondenti, ma subisce una progressiva contaminazione (\emph{pollution}) dovuta a due fonti principali di \textbf{impurezze}:
\begin{enumerate}
    \item \textbf{Ceneri di fusione ($\text{He}^4$)}: i nuclei di elio prodotti dalla reazione $\text{D}-\text{T}$, una volta ceduta la loro energia, rimangono nel plasma come impurezze con numero atomico $Z_{\text{He}} = 2$.
    \item \textbf{Materiali di parete (es. Carbonio $\text{C}$ con $Z_{\text{C}} = 6$, Neon $\text{Ne}$ con $Z_{\text{Ne}} = 10$, o Tungsteno $\text{W}$)}: gli ioni energetici e i neutroni che colpiscono la prima parete del \emph{vacuum vessel} (spesso rivestita in tungsteno $\text{W}$) estraggono atomi pesanti che penetrano nel plasma e si ionizzano.
\end{enumerate}

\begin{figure}[htbp]
\centering
\begin{tikzpicture}[scale=0.95]
    % Parete del Tokamak (Vessel in Tungsteno W)
    \draw[line width=3pt, gray!70!black, fill=gray!10] (0,0) rectangle (6.2, 3.6);
    \node[above] at (3.1, 3.6) {\textbf{TOKAMAK VESSEL (made of Tungsten W)}};

    % Particelle nel plasma inquinato
    \node[font=\bfseries, red!80!black] at (1.0, 2.8) {$\text{D}^+$};
    \node[font=\bfseries, red!80!black] at (4.5, 2.9) {$\text{T}^+$};
    \node[font=\bfseries, red!80!black] at (1.8, 1.6) {$\text{T}^+$};
    \node[font=\bfseries, red!80!black] at (4.2, 1.2) {$\text{D}^+$};

    \node[font=\bfseries, blue!80!black] at (2.8, 3.0) {$e^-$};
    \node[font=\bfseries, blue!80!black] at (3.8, 2.1) {$e^-$};
    \node[font=\bfseries, blue!80!black] at (1.0, 0.9) {$e^-$};
    \node[font=\bfseries, blue!80!black] at (3.1, 0.5) {$e^-$};
    \node[font=\bfseries, blue!80!black] at (2.3, 2.3) {$e^-$};

    % Impurezze (He-4 e W)
    \node[font=\bfseries, green!50!black, draw=green!50!black, circle, inner sep=1pt] at (5.1, 0.6) {$\text{He}^{2+}$};
    \node[font=\bfseries, purple!80!black, draw=purple!80!black, rectangle, inner sep=2pt] at (5.3, 2.0) {$\text{W}^{Z+}$};

    % Frecce esplicative esterne
    \draw[-{Stealth}, thick, green!50!black] (5.6, 0.6) -- (7.2, 0.6) node[right, text width=4.8cm, font=\small, black] {$\text{He}^4$ ash ($Z_{\text{He}}=2$): considered as an impurity ($\rightarrow$ plasma gets polluted)};
    \draw[-{Stealth}, thick, purple!80!black] (5.7, 2.0) -- (7.2, 2.0) node[right, text width=4.8cm, font=\small, black] {Heavy wall impurities sputtered from the vessel (e.g.\ $\text{W}, \text{C}$)};
\end{tikzpicture}
\caption{Rappresentazione schematica di un plasma contaminato da ceneri di elio ($\text{He}^{2+}$) e impurezze rilasciate dalla parete della camera da vuoto ($\text{W}$).}
\label{fig:plasma_impurities}
\end{figure}

Mentre gli ioni di combustibile $\text{D}^+$ e $\text{T}^+$ hanno carica unitaria ($Z_D = Z_T = 1$), l'$i$-esima specie di impurezza con densità numerica $n_i$ possiede carica $Z_i > 1$ e rilascia $Z_i$ elettroni per ogni ione. La nuova condizione di quasi-neutralità con $N_{imp}$ specie inquinanti diventa pertanto:
\begin{equation}
    n_{e,imp} = n_D + n_T + \sum_{i=1}^{N_{imp}} Z_i n_i
\end{equation}
Assumendo la densità elettronica $n_e$ come densità di riferimento, si definisce la \textbf{concentrazione della $i$-esima impurezza} come:
\begin{equation}
    c_i = \frac{n_i}{n_e} \implies \sum_{i=1}^{N_{imp}} Z_i n_i = n_e \sum_{i=1}^{N_{imp}} Z_i c_i
\end{equation}
Per una miscela di combustibile equimolare ($n_D = n_T$), l'equazione di bilancio delle cariche si riscrive come:
\begin{equation}
    n_e = 2 n_D + n_e \sum_{i} Z_i c_i \implies n_e \left( 1 - \sum_{i} Z_i c_i \right) = 2 n_D
\end{equation}
da cui si ricava il legame tra la densità elettronica e la densità di ioni di Deuterio:
\begin{equation}
    n_e = \frac{2 n_D}{1 - \sum_{i} Z_i c_i}
    \label{eq:ne_impurity}
\end{equation}

\subsection{Derivazione della Riduzione di Potenza di Fusione}
La presenza di impurezze altera non solo la densità elettronica $n_e$, ma anche la pressione del plasma $p_{imp}$. Seguendo la derivazione svolta a lezione (Pagine 30--31), esprimiamo la pressione associata alla densità elettronica in presenza di impurezze come:
\begin{equation}
    p_{imp} = \left( 2 n_D + n_e \sum_{i} Z_i c_i \right) T
\end{equation}
Sostituendo l'espressione di $n_e$ data dall'Equazione~\eqref{eq:ne_impurity}:
\begin{equation}
    p_{imp} = \left[ 2 n_D \left( 1 + \frac{\sum_{i} Z_i c_i}{1 - \sum_{i} Z_i c_i} \right) \right] T
\end{equation}
Poiché in un reattore a confinamento magnetico esiste un limite tecnologico e magnetoidrodinamico invalicabile sulla pressione massima sostenibile dal campo magnetico ($p \le p_{lim}$, data dal contributo di ioni ed elettroni), confrontiamo il plasma inquinato e il plasma puro a \textbf{parità di pressione $p$ e di temperatura $T$}. Esplicitando la densità del combustibile $n_{D,imp}$ nel caso contaminato rispetto al caso puro ($n_D = \frac{p}{2T}$ nella medesima normalizzazione):
\begin{equation}
    n_{D,imp} = \frac{p}{2T} \left[ 1 + \frac{\sum_{i} Z_i c_i}{1 - \sum_{i} Z_i c_i} \right]^{-1}
\end{equation}
Calcoliamo ora il rapporto tra la densità di potenza di fusione prodotta in presenza di impurezze ($W_{int,i} = W_{fus}^{imp}$) e quella prodotta nel plasma ideale puro ($W_{int} = W_{fus}$):
\begin{align}
    \frac{W_{int,i}}{W_{int}} &= \frac{\cancel{E_\alpha}\, n_{D,imp}^2\, \cancel{\langle \sigma v \rangle_{DT}}}{\cancel{E_\alpha}\, n_D^2\, \cancel{\langle \sigma v \rangle_{DT}}} = \frac{\cancel{\dfrac{p^2}{4 T^2}} \left[ 1 + \dfrac{\sum_{i} Z_i c_i}{1 - \sum_{i} Z_i c_i} \right]^{-2}}{\cancel{\dfrac{p^2}{4 T^2}}} \nonumber \\[0.2cm]
    &= \left[ 1 + \frac{\sum_{i} Z_i c_i}{1 - \sum_{i} Z_i c_i} \right]^{-2} = \left( 1 - \sum_{i} Z_i c_i \right)^2 < 1
\end{align}
(Nota: tenendo conto anche del contributo diretto della densità ionica nella pressione totale $p_{imp} = (n_e + n_D + n_T + \sum_i n_i)T$, il fattore correttivo diventa $\left[ 1 + \frac{\sum_i (Z_i + 1)c_i}{2(1 - \sum_i Z_i c_i)} \right]^{-2}$, che presenta il medesimo andamento monotono decrescente). In entrambi i casi si dimostra che:
\begin{equation}
    W_{int,i} < W_{int} \quad \left(\text{ovvero } W_{fus}^{imp} < W_{fus}\right)
\end{equation}
La produzione di potenza di fusione diminuisce drasticamente a causa della diluizione del combustibile indotta dalle impurezze: poiché non si può superare la pressione limite $p_{lim}$, se sono presenti troppi inquinanti la pressione parziale degli elettroni e delle impurezze aumenta, sottraendo margine alla pressione parziale del combustibile utile $\text{D}-\text{T}$.

\subsection{Impatto sul Criterio di Lawson e sul Prodotto Triplo}
Come illustrato nei grafici di Figura~\ref{fig:impurity_plots}:
\begin{itemize}
    \item All'aumentare della concentrazione di contaminazione $c$, il rapporto $\frac{W_{fus}^{imp}}{W_{fus}}$ crolla rapidamente verso zero.
    \item All'aumentare del numero atomico $Z_i$ dell'inquinante (passando da $Z=2$ per l'Elio, a $Z=6$ per il Carbonio, fino a $Z=10$ per il Neon o $Z=74$ per il Tungsteno), la potenza di fusione scende molto più velocemente a parità di concentrazione $c$.
    \item Sul piano $(T, n_e \tau_E)$, a parità di temperatura $T$ si ha $p_{imp} > p$ e quindi il parametro di confinamento richiesto per l'ignizione nel caso reale risulta superiore a quello ideale:
    \begin{equation}
        (p \tau_E)_{imp} > (p \tau_E)_{ideal} \iff (n_e \tau_E)_{reality} > (n_e \tau_E)_{ideal}
    \end{equation}
    La curva di ignizione reale si sposta verso l'alto rispetto alla curva ideale, richiedendo valori più elevati del prodotto triplo $n_e T \tau_E$ e individuando un preciso intervallo di temperature $T$ per ottenere un buon punto di lavoro.
\end{itemize}

\begin{figure}[htbp]
\centering
\begin{tikzpicture}
    % --- Grafico Sinistro: W_fus^imp / W_fus in funzione di c ---
    \begin{axis}[
        name=plot1,
        width=7.5cm,
        height=6.8cm,
        xmin=0, xmax=0.31,
        ymin=0, ymax=1.05,
        xlabel={$c$ (Contaminazione)},
        ylabel={$\dfrac{W_{fus}^{imp}}{W_{fus}}$},
        grid=both,
        grid style={line width=.1pt, draw=gray!25},
        legend pos=north east,
        legend style={font=\footnotesize},
        tick label style={font=\footnotesize}
    ]
        % Curva Z=2 (He)
        \addplot[very thick, blue!70!cyan, line width=2pt, domain=0:0.30, samples=50]
            {((1 - 2*x)/(1 - 0.5*x))^2};
        \addlegendentry{$Z=2\ (\text{He})$}

        % Curva Z=6 (C)
        \addplot[very thick, orange!90!red, line width=2pt, domain=0:0.15, samples=50]
            {((1 - 6*x)/(1 - 2.5*x))^2};
        \addlegendentry{$Z=6\ (\text{C})$}

        % Curva Z=10 (Ne)
        \addplot[very thick, green!60!black, line width=2pt, domain=0:0.08, samples=50]
            {((1 - 10*x)/(1 - 4.5*x))^2};
        \addlegendentry{$Z=10\ (\text{Ne})$}
    \end{axis}

    % --- Grafico Destro: Curva di Lawson n_e * tau_E vs T (Ideal vs Reality) ---
    \begin{scope}[shift={(8.3, 0.4)}]
        \draw[-{Stealth[length=3mm]}, very thick] (0,0) -- (6.2,0) node[below left, font=\bfseries] {$T$};
        \draw[-{Stealth[length=3mm]}, very thick] (0,0) -- (0,5.2) node[above left, rotate=90, font=\bfseries] {$n_e \tau_E$};

        % Curva IDEAL (in basso, azzurra)
        \draw[very thick, cyan!70!blue, line width=1.6pt] (0.5, 4.4) .. controls (1.5, 0.6) and (3.8, 0.6) .. (5.2, 3.0);
        % Tratteggio regione ideale
        \foreach \x in {1.0, 1.6, 2.2, 2.8, 3.4, 4.0, 4.6} {
            \draw[cyan!70!blue, thin] (\x, {0.32*(\x-2.85)^2 + 1.45}) -- (\x-0.2, 4.3);
        }
        \draw[-{Stealth}, thick, cyan!70!blue] (4.7, 2.0) -- (5.6, 1.8) node[right, font=\bfseries\small] {IDEAL};

        % Curva REALITY (più in alto, arancione)
        \draw[very thick, orange!90!red, line width=1.6pt] (0.8, 4.8) .. controls (1.8, 1.6) and (3.8, 1.6) .. (5.5, 3.8);
        % Tratteggio regione reale
        \foreach \x in {1.3, 1.9, 2.5, 3.1, 3.7, 4.3, 4.9} {
            \draw[orange!90!red, thin] (\x, {0.32*(\x-2.95)^2 + 2.3}) -- (\x-0.3, 4.7);
        }
        \draw[-{Stealth}, thick, red!80!black] (4.3, 3.8) -- (5.6, 4.2) node[right, font=\bfseries\small] {REALITY};

        % Nota sul range di temperatura ottimale
        \node[below, text width=5.5cm, align=center, font=\scriptsize\itshape] at (3.1, 0.8) {
            Serve una $T \in \text{range}$ ottimale per avere un buon prodotto triplo $(n_e T \tau_E)$.
        };
    \end{scope}
\end{tikzpicture}
\caption{A sinistra: decadimento della densità di potenza di fusione normalizzata $W_{fus}^{imp}/W_{fus}$ in funzione della concentrazione di impurezze $c$ per $Z=2\ (\text{He})$, $Z=6\ (\text{C})$ e $Z=10\ (\text{Ne})$. A destra: confronto qualitativo nel piano $(T, n_e \tau_E)$ tra la curva di ignizione ideale e quella reale in presenza di impurezze.}
\label{fig:impurity_plots}
\end{figure}

% !TeX program = xelatex





% ============================================================================
\section{Main Objectives and Course Outline}
% ============================================================================

\subsection{Main Objectives}
\begin{enumerate}
    \item So far we have characterized the plasma in a qualitative way.
    \item Let's try now to give a more quantitative description.
    \item We will start from already introduced basic concepts and derive quantitative parameters to discriminate an ionized gas from a real plasma.
\end{enumerate}

\begin{figure}[htbp]
\centering
\begin{tikzpicture}[>=Stealth, scale=0.95, every node/.style={transform shape}]
    % Stage 1
    \node[draw=red!80!black, circle, thick, inner sep=2pt, font=\bfseries, text=red!80!black] at (-0.6, 3.2) {1};
    \draw[red!80!black, very thick] (0,0) rectangle (3,3);
    \node[green!60!black, font=\bfseries\large] at (0.6, 2.2) {D};
    \node[cyan!70!blue, font=\bfseries\large] at (1.0, 1.4) {T};
    \node[green!60!black, font=\bfseries\large] at (1.7, 1.6) {D};
    \node[cyan!70!blue, font=\bfseries\large] at (2.3, 2.1) {T};
    \node[green!60!black, font=\bfseries\large] at (0.8, 0.6) {D};
    \node[cyan!70!blue, font=\bfseries\large] at (2.1, 0.7) {T};

    % Arrow 1 -> 2
    \draw[->, magenta, ultra thick] (3.1, 1.5) -- (3.8, 1.5);
    \node[red!85!black, font=\bfseries\large] at (4.65, 1.65) {10 keV};
    \draw[red!85!black, thick] (3.95, 1.35) -- (5.35, 1.35);
    \draw[->, magenta, ultra thick] (5.5, 1.5) -- (6.2, 1.5);

    % Stage 2
    \node[draw=red!80!black, circle, thick, inner sep=2pt, font=\bfseries, text=red!80!black] at (5.9, 3.3) {2};
    \draw[red!80!black, very thick] (6.5,0) rectangle (9.8,3);
    \node[red!80!black, font=\bfseries, align=center] at (8.15, -0.6) {FULLY-IONIZED\\GAS};

    % Orange collision highlight in Box 2
    \draw[orange!90!black, very thick] (7.6, 2.45) ellipse (0.95cm and 0.42cm);
    \node[green!60!black, font=\bfseries] at (7.05, 2.45) {$D^+$};
    \node[cyan!70!blue, font=\bfseries] at (8.15, 2.40) {$T^+$};

    % Other particles in Box 2
    \node[red!85!black, font=\bfseries] at (7.0, 1.6) {$e^-$};
    \node[red!85!black, font=\bfseries] at (7.9, 1.7) {$e^-$};
    \node[red!85!black, font=\bfseries] at (8.6, 1.9) {$e^-$};
    \node[cyan!70!blue, font=\bfseries] at (9.2, 2.2) {$T^+$};
    \node[green!60!black, font=\bfseries] at (7.4, 1.0) {$D^+$};
    \node[green!60!black, font=\bfseries] at (8.9, 1.1) {$D^+$};
    \node[red!85!black, font=\bfseries] at (7.2, 0.4) {$e^-$};
    \node[red!85!black, font=\bfseries] at (7.7, 0.4) {$e^-$};
    \node[cyan!70!blue, font=\bfseries] at (8.3, 0.6) {$T^+$};
    \node[red!85!black, font=\bfseries] at (9.1, 0.4) {$e^-$};

    % "IF THEY COLLIDE" annotation
    \node[orange!90!black, font=\bfseries\small, align=center] at (9.8, 3.65) {IF THEY\\COLLIDE};
    \draw[->, orange!90!black, very thick] (7.9, 2.85) to[out=60, in=180] (8.8, 3.55);
    \draw[->, orange!90!black, very thick] (10.7, 3.3) -- (11.1, 2.8);

    % Arrow 2 -> 3
    \draw[->, magenta, ultra thick] (10.0, 1.6) -- (10.9, 1.6);

    % Stage 3
    \draw[red!80!black, very thick] (11.2,0) rectangle (14.6,3);
    \draw[orange!90!black, very thick] (12.15, 2.05) circle (0.85cm);
    \node[green!60!black, font=\bfseries] at (11.75, 2.35) {$\text{He}^4$};
    \node[black, font=\bfseries] at (11.8, 1.6) {$n$};
    \node[red!85!black, font=\bfseries] at (12.55, 2.25) {$e^-$};
    \node[red!85!black, font=\bfseries] at (12.45, 1.65) {$e^-$};

    % Other particles in Box 3
    \node[green!60!black, font=\bfseries] at (13.6, 2.4) {$D^+$};
    \node[red!85!black, font=\bfseries] at (14.2, 2.5) {$e^-$};
    \node[red!85!black, font=\bfseries] at (14.1, 1.8) {$e^-$};
    \node[cyan!70!blue, font=\bfseries] at (11.8, 0.6) {$T^+$};
    \node[cyan!70!blue, font=\bfseries] at (12.7, 0.8) {$T^+$};
    \node[green!60!black, font=\bfseries] at (13.7, 1.0) {$D^+$};
    \node[red!85!black, font=\bfseries] at (13.2, 0.4) {$e^-$};
    \node[red!85!black, font=\bfseries] at (14.1, 0.5) {$e^-$};

    % Stage 3 label and Burning Plasma arrow
    \node[draw=red!80!black, circle, thick, inner sep=2pt, font=\bfseries, text=red!80!black] at (15.1, 2.8) {3};
    \draw[->, magenta, very thick] (13.0, 2.3) to[out=55, in=160] (16.5, 3.2);
    \node[magenta!90!black, font=\bfseries, align=center] at (16.6, 2.3) {BURNING\\PLASMA};
\end{tikzpicture}
\caption{Schema manoscritto dell'evoluzione da un gas neutro di Deuterio ($D$) e Trizio ($T$) riscaldato a $10\text{ keV}$ fino allo stato di gas completamente ionizzato ($D^+, T^+, e^-$) e, tramite collisioni di fusione con produzione di particelle alfa ($\text{He}^4$) e neutroni ($n$), a un \textit{Burning Plasma}.}
\end{figure}



% ============================================================================
\section{From Kinetic to Fluid Treatment of the Plasma}
% ============================================================================

\subsection{Single-Particle Motion vs.\ Statistical Kinetic Theory}
\begin{itemize}
    \item \textbf{Plasma is a fully-ionized gas with free charges} $\longrightarrow$ costituito da elettroni liberi $e^-$ e ioni positivi $i^+$.
    \item \textbf{Particles interact with Coulomb collisions.}
    \begin{quote}
        \textit{Appunto manoscritto:} Le particelle cambiano la loro traiettoria perch\'e sono soggette a forze elettrostatiche attrattive e repulsive ($F_{\text{ATTR}}, F_{\text{REPULS}}$). Non \`e necessario un urto fisico diretto (``hit'') come nelle sfere rigide: l'interazione coulombiana \`e a lungo raggio.
    \end{quote}
    \item \textbf{These particles are in an electro-magnetized environment} $\longrightarrow$ \textit{can we use Newton equation to know their trajectories?}
    \begin{equation}
        m \cdot \mathbf{a} = q(\mathbf{E} + \mathbf{v} \times \mathbf{B})
    \end{equation}
    \begin{quote}
        \textit{Appunto manoscritto:} Ioni ($i^+$) ed elettroni ($e^-$) si muovono in modo diverso sotto l'azione dei campi perch\'e hanno masse molto diverse ($m_e \neq m_{\text{ION}}$, con $m_e \ll m_i$).
    \end{quote}
    \item \textbf{Limitation of single-particle tracking:} L'equazione di Newton \`e esatta per una singola particella, ma in un plasma tipico da fusione abbiamo circa:
    \begin{equation}
        n \approx 10^{20}\text{ part/m}^3
    \end{equation}
    \`E quindi impossibile risolvere $10^{20}$ equazioni del moto accoppiate.
    \item \textbf{Behavior of particles in a plasma follows the kinetic theory of Maxwell-Boltzmann:}
    \begin{itemize}
        \item Random-walk theory
        \item Gaussian distribution
    \end{itemize}
\end{itemize}

\begin{figure}[htbp]
\centering
\begin{tikzpicture}[>=Stealth, scale=0.9, every node/.style={transform shape}]
    % Base platform
    \fill[gray!35] (0,0) rectangle (14.5, 0.3);
    \draw[gray!70!black, thick] (0,0) rectangle (14.5, 0.3);

    % --- Cube 1: PLASMA ---
    \node[font=\bfseries\sffamily] at (2.2, 4.3) {PLASMA};
    \fill[gray!15, fill opacity=0.7] (0.5, 0.3) rectangle (3.5, 3.3);
    \draw[gray!70, thick] (0.5, 0.3) rectangle (3.5, 3.3);
    \draw[gray!70, thick] (0.5, 3.3) -- (1.2, 3.8) -- (4.2, 3.8) -- (3.5, 3.3);
    \draw[gray!70, thick] (3.5, 0.3) -- (4.2, 0.8) -- (4.2, 3.8);
    % Ions (+) and Electrons (-)
    \foreach \x/\y in {1.2/0.9, 1.8/1.4, 1.9/2.3, 2.8/2.8, 3.6/1.4} {
        \fill[cyan!60!blue] (\x,\y) circle (0.26cm);
        \node[white, font=\bfseries\small] at (\x,\y) {$+$};
    }
    \foreach \x/\y in {0.9/2.2, 1.7/3.3, 2.7/1.8, 3.0/0.8, 3.7/2.5} {
        \fill[orange!90!yellow] (\x,\y) circle (0.24cm);
        \node[white, font=\bfseries\small] at (\x,\y) {$-$};
    }

    % --- Cube 2: GAS ---
    \node[font=\bfseries\sffamily] at (7.3, 4.3) {GAS};
    \fill[cyan!10, fill opacity=0.7] (5.5, 0.3) rectangle (8.5, 3.3);
    \draw[cyan!50!gray, thick] (5.5, 0.3) rectangle (8.5, 3.3);
    \draw[cyan!50!gray, thick] (5.5, 3.3) -- (6.2, 3.8) -- (9.2, 3.8) -- (8.5, 3.3);
    \draw[cyan!50!gray, thick] (8.5, 0.3) -- (9.2, 0.8) -- (9.2, 3.8);
    \foreach \x/\y in {6.1/0.8, 7.5/0.9, 6.7/1.4, 8.3/1.5, 6.2/2.1, 7.1/2.4, 8.0/2.6, 6.0/2.9, 7.6/3.2, 8.7/2.2} {
        \fill[cyan!70!blue!80] (\x,\y) circle (0.32cm);
    }

    % --- Cube 3: SOLID ---
    \node[font=\bfseries\sffamily] at (12.3, 4.3) {SOLID};
    \fill[cyan!25, fill opacity=0.7] (10.5, 0.3) rectangle (13.5, 3.3);
    \draw[cyan!60!gray, thick] (10.5, 0.3) rectangle (13.5, 3.3);
    \draw[cyan!60!gray, thick] (10.5, 3.3) -- (11.2, 3.8) -- (14.2, 3.8) -- (13.5, 3.3);
    \draw[cyan!60!gray, thick] (13.5, 0.3) -- (14.2, 0.8) -- (14.2, 3.8);
    \foreach \x in {11.0, 12.0, 13.0} {
        \foreach \y in {0.8, 1.8, 2.8} {
            \fill[cyan!75!blue!90] (\x,\y) circle (0.45cm);
        }
    }

    % Bottom Density Arrow
    \node[font=\sffamily\small] at (1.5, -0.35) {Low density};
    \node[font=\sffamily\small] at (12.0, -0.35) {High density};
    \draw[<->, ultra thick, blue!70!black] (0.2, -0.8) -- (7.25, -0.8);
    \draw[->, ultra thick, red!80!black] (7.25, -0.8) -- (14.3, -0.8);
\end{tikzpicture}
\caption{Confronto qualitativo tra gli stati della materia (Plasma, Gas, Solido) al variare della densit\`a.}
\end{figure}

\subsection{Phase Space and Maxwellian Distribution Function}
Per descrivere statisticamente il sistema introduciamo la \textbf{funzione di distribuzione} nello spazio delle fasi (\textit{Phase Space}):
\begin{equation}
    f(\mathbf{r}, \mathbf{v}, t) \quad \text{con} \quad
    \begin{cases}
        \mathbf{r} = (x, y, z) & \text{coordinate spaziali (3D)} \\
        \mathbf{v} = (v_x, v_y, v_z) & \text{coordinate di velocit\`a (3D)} \\
        t & \text{tempo (variazione temporale, 1D)}
    \end{cases}
\end{equation}
La funzione di distribuzione $f(\mathbf{r}, \mathbf{v}, t)$ dipende quindi da \textbf{6D + tempo (7 variabili indipendenti)} per ciascuna specie di elettroni $e^-$ o ioni $i^+$ considerata.

Se il sistema \`e vicino all'equilibrio termodinamico (\textit{appunto: condizione stazionaria}), la distribuzione di velocit\`a delle particelle assume una \textbf{forma Maxwelliana}:
\begin{equation}
    f(v) \propto \exp\left( -\frac{\frac{1}{2} m v^2}{k_B T} \right)
\end{equation}
\textit{Significato fisico (dagli appunti):} Questa descrizione statistica sottolinea che dobbiamo considerare un insieme di moltissime particelle (\textit{``we have to consider many particles, not only one''}), la maggior parte delle quali si muove con velocit\`a prossima al valore medio $v = v_0$.

\begin{figure}[htbp]
\centering
\begin{tikzpicture}
    \begin{axis}[
        width=9cm, height=5cm,
        axis lines=middle,
        xlabel={$v$},
        ylabel={$f(v)$},
        xmin=-3.2, xmax=3.2,
        ymin=0, ymax=1.2,
        xtick={-1.5, 0, 1.5},
        xticklabels={$-v_1$, $v_0$, $v_1$},
        ytick=\empty,
        samples=100,
        domain=-3:3
    ]
        \addplot[green!60!black, ultra thick] {exp(-x^2)};
        \draw[dashed, thick, gray!70!black] (axis cs:0,0) -- (axis cs:0,1.0);
        \draw[dotted, thick, gray!70!black] (axis cs:-1.5,0) -- (axis cs:-1.5,0.105);
        \draw[dotted, thick, gray!70!black] (axis cs:1.5,0) -- (axis cs:1.5,0.105);
    \end{axis}
\end{tikzpicture}
\caption{Distribuzione gaussiana/maxwelliana delle velocit\`a $f(v)$ centrata attorno alla velocit\`a media $v_0$.}
\end{figure}

\subsection{The Kinetic Equation of Vlasov/Boltzmann}
L'evoluzione spazio-temporale della funzione di distribuzione $f_\alpha(\mathbf{r}, \mathbf{v}, t)$ per la specie $\alpha$ nello spazio 7D $(\mathbf{r}, \mathbf{v}, t)$ \`e governata dall'\textbf{equazione cinetica di Vlasov/Boltzmann}:
\begin{equation}
    \underbrace{\frac{\partial f_\alpha}{\partial t}}_{\substack{\text{Rate of change}\\\text{of the distribution}\\\text{in time}}}
    + \underbrace{\mathbf{v} \cdot \nabla f_\alpha}_{\substack{\text{Convection:}\\\text{change with }\mathbf{r}}}
    + \underbrace{\frac{q_\alpha}{m_\alpha} \big( \overbrace{\mathbf{E}}^{\text{Electrostatic}} + \overbrace{\mathbf{v} \times \mathbf{B}}^{\text{Magnetic fields}} \big) \cdot \nabla_{\mathbf{v}} f_\alpha}_{\substack{\text{Forza elettromagnetica:}\\\text{change w.r.t.\ }\mathbf{v}}}
    = \underbrace{\left( \frac{\delta f_\alpha}{\delta t} \right)_{\text{coll}}}_{\substack{\text{Collision:}\\\text{source of variation}\\\text{due to collisions}}}
\end{equation}

\subsection{Moment Reduction: From Kinetic Equation to Fluid Equations}
Possiamo ridurre la complessit\`a di questo sistema 7D considerando il comportamento macroscopico delle particelle:
\begin{itemize}
    \item Calcolando i \textbf{momenti di ordine $0^{\text{th}}$, $1^\circ$ e $2^\circ$} della funzione di distribuzione $f(\mathbf{r}, \mathbf{v}, t)$ rispetto allo spazio delle velocit\`a (\textit{appunto: integrando eliminiamo la variabile $\mathbf{v}$, dove $d^3v = dv_x dv_y dv_z$}), si ottengono le \textbf{equazioni fluide}!
\end{itemize}

\begin{center}
\renewcommand{\arraystretch}{1.8}
\begin{tabular}{|c|c|c|}
\hline
\textbf{Moment} & \textbf{Integration operator} \textit{(elimina $\mathbf{v}$)} & \textbf{Result \& Physical Meaning} \\
\hline
$0^{\text{th}}: M_0$ & $\displaystyle \int f(\mathbf{r}, \mathbf{v}, t)\, \underbrace{d^3v}_{dv_x dv_y dv_z}$ & $M_0 = n(\mathbf{r}, t)$ \quad (\textbf{Density}) \\
\hline
$1^\circ: \mathbf{M}_1$ & $\displaystyle \int \mathbf{v}\, f(\mathbf{r}, \mathbf{v}, t)\, d^3v$ & $\mathbf{M}_1 = n(\mathbf{r}, t)\mathbf{u}(\mathbf{r}, t)$ \quad (\textbf{Momentum}) \\
\hline
$2^\circ: M_2$ & $\displaystyle \int \frac{1}{2} m_\alpha v^2 f(\mathbf{r}, \mathbf{v}, t)\, d^3v$ & $M_2 = \dfrac{3 p(\mathbf{r}, t)}{m} + n(\mathbf{r}, t) u^2(\mathbf{r}, t)$ \quad (\textbf{Energy}) \\
\hline
\end{tabular}
\end{center}

\noindent\textit{Osservazioni dagli appunti manoscritti:}
\begin{itemize}
    \item Dal momento del primo ordine $\mathbf{M}_1$, poich\'e $\mathbf{v} = (v_x, v_y, v_z)$ \`e un vettore tridimensionale e la distribuzione gaussiana \`e in 3D, otteniamo \textbf{3 diversi momenti del $1^\circ$ ordine} (uno per ciascuna direzione spaziale), che conducono all'equazione di conservazione della quantit\`a di moto (analoga a \textbf{Navier-Stokes}).
\end{itemize}

\begin{figure}[htbp]
\centering
\begin{tikzpicture}[
    >=Stealth,
    node distance=1.0cm and 1.2cm,
    box/.style={draw=blue!70!black, rounded corners=4pt, fill=blue!5, thick, align=center, inner sep=6pt, font=\small},
    highlight/.style={draw=red!80!black, rounded corners=4pt, fill=red!5, thick, align=center, inner sep=6pt, font=\small\bfseries}
]
    \node[highlight] (single) {SINGLE PARTICLE\\$\mathbf{m\mathbf{a} = q(\mathbf{E} + \mathbf{v}\times\mathbf{B})}$\\(Newton Equation)};
    \node[box, below=0.9cm of single] (many) {But we have many particles (GAS)\\$\Downarrow$\\KINETIC THEORY OF GAS: $f(\mathbf{r}, \mathbf{v}, t)$};
    \node[highlight, below=0.9cm of many] (vlasov) {VLASOV EQUATION\\(7D System: $\mathbf{r}, \mathbf{v}, t$)};
    \node[box, right=1.4cm of vlasov] (macro) {DECREASE COMPLICATION\\OF 7D SYSTEM\\(Macroscopic Way)};
    \node[box, above=0.9cm of macro] (moments) {$0^{\text{th}}, 1^\circ, 2^\circ$ ORDER MOMENTS\\of $f(\mathbf{r}, \mathbf{v}, t)$};
    \node[highlight, above=0.9cm of moments] (fluid) {GENERALIZING THE GAS AS A FLUID\\Conservation Equations:\\$\bullet$ Density ($M_0$)\\$\bullet$ Momentum ($\mathbf{M}_1$)\\$\bullet$ Energy ($M_2$)};

    \draw[->, very thick] (single) -- (many);
    \draw[->, very thick] (many) -- (vlasov);
    \draw[->, very thick] (vlasov) -- (macro);
    \draw[->, very thick] (macro) -- (moments);
    \draw[->, very thick] (moments) -- (fluid);
\end{tikzpicture}
\caption{Mappa concettuale riassuntiva (\textit{``Small resume of what we have did''}): dal moto di singola particella alla descrizione fluida tramite i momenti dell'equazione di Vlasov.}
\end{figure}

\subsection{Knudsen Number and Condition for Fluid Treatment}
La condizione fisica per poter applicare il trattamento fluido a un sistema di particelle \`e definita tramite il \textbf{numero di Knudsen} ($Kn$):
\begin{equation}
    Kn = \frac{\lambda_{\text{mfp}}}{L}
\end{equation}
dove:
\begin{itemize}
    \item $\lambda_{\text{mfp}}$ \`e il \textbf{cammino libero medio} (\textit{Mean free path between collisions}, ovvero la distanza media percorsa da una particella tra una collisione coulombiana e un'altra). Come annotato a mano, nelle collisioni coulombiane non \`e necessario un contatto diretto (``hit''), poich\'e si tratta di un'interazione elettrostatica attrattiva o repulsiva a distanza.
    \item $L$ \`e la \textbf{lunghezza caratteristica del sistema} (\textit{Characteristic length of the system}, cio\`e quanto \`e grande il sistema fisico in esame).
\end{itemize}

Si distinguono due regimi:
\begin{equation}
    \begin{cases}
        \mathbf{Kn \ll 0.01}: & \text{collisions are frequent w.r.t.\ length } L \implies \textbf{Fluid description} \\
        \mathbf{Kn \gg 10}: & \text{collisions are not frequent w.r.t.\ length } L \implies \textbf{Kinetic description}
    \end{cases}
\end{equation}
\begin{quote}
    \textit{Appunti manoscritti:} Il regime $Kn \ll 0.01$ garantisce l'\textbf{uniformit\`a della funzione di distribuzione} e consente di trattare il plasma come un \textbf{modello a 2 fluidi} ($i^+, e^-$). Questa \`e la condizione tipica di un plasma!
\end{quote}

\subsection{Remarks: Standard Fluid vs.\ Plasma}
\textit{Is a plasma a simple fluid? Which are the main differences with a standard fluid?}

\begin{center}
\renewcommand{\arraystretch}{1.5}
\begin{tabular}{|p{6.5cm}|p{6.5cm}|}
\hline
\textbf{Standard fluid} & \textbf{Plasma} \\
\hline
Neutral particles & Charged particles \\
\hline
Singular molecular species & Two-fluid species ($e^-$ and $i^+$) \\
\hline
Short-range collisions & Long-range collisions (Coulomb) \\
\hline
No collective electromagnetic parameters & Characteristic values of $\lambda_D, \omega_p, \Lambda$ \\
\hline
\end{tabular}
\end{center}

\noindent\textit{Nota manoscritta sulla differenza di specie:} In un fluido ordinario come l'acqua ($\text{H}_2\text{O}$), gli atomi di $\text{H}$ e $\text{O}$ sono legati e hanno propriet\`a (come la massa) dello stesso ordine di grandezza; nel plasma, invece, elettroni $e^-$ e ioni $i^+$ sono separati e hanno propriet\`a fisiche (in particolare l'inerzia $m_e \ll m_i$) profondamente diverse, rendendo necessaria una descrizione a \textbf{due fluidi}.

% ============================================================================
\section{Debye Length ($\lambda_D$) and Debye Shielding}
% ============================================================================

\subsection{System Under Analysis and Physical Assumptions}
Consideriamo un plasma confinato tra due pareti piane parallele infinite poste a distanza $L$ (in $x = -L/2$ e $x = +L/2$, geometria 1D \textit{slab}), alle quali \`e applicata una piccola differenza di potenziale $\Delta V$.

\begin{figure}[htbp]
\centering
\begin{tikzpicture}[>=Stealth, scale=0.95, every node/.style={transform shape}]
    % Left wall at -L/2
    \draw[very thick] (-3.5, -2.2) -- (-3.5, 2.2);
    \foreach \y in {-1.8, -1.2, -0.6, 0.0, 0.6, 1.2, 1.8} {
        \draw[thick] (-3.5, \y) -- (-4.1, \y-0.4);
        \node[green!60!black, font=\bfseries\large] at (-4.6, \y) {$+$};
    }
    \node[font=\bfseries] at (-3.5, -2.6) {$-L/2$};

    % Right wall at +L/2
    \draw[very thick] (3.5, -2.2) -- (3.5, 2.2);
    \foreach \y in {-1.8, -1.2, -0.6, 0.0, 0.6, 1.2, 1.8} {
        \draw[thick] (3.5, \y) -- (4.1, \y-0.4);
        \node[red!80!black, font=\bfseries\large] at (4.6, \y) {$-$};
    }
    \node[font=\bfseries] at (3.5, -2.6) {$L/2$};

    % Centerline and Origin
    \draw[dashed, thick] (-3.5, 0) -- (3.5, 0);
    \draw[thick] (0, -0.2) -- (0, 0.2);
    \node[below] at (0, -0.2) {$0$};

    % Potential arc at top
    \draw[blue!70!black, thick] (-3.5, 2.4) to[out=15, in=165] node[above, font=\bfseries] {$\Delta V$} (3.5, 2.4);

    % Particles inside
    \node[red!85!black, font=\bfseries\large] at (-2.0, 1.0) {$e^-$};
    \node[green!60!black, font=\bfseries\large] at (-0.8, 1.4) {$i^+$};
    \node[red!85!black, font=\bfseries\large] at (1.5, 1.2) {$e^-$};
    \node[green!60!black, font=\bfseries\large] at (-2.1, -1.1) {$i^+$};
    \node[red!85!black, font=\bfseries\large] at (-0.6, -1.2) {$e^-$};
    \node[green!60!black, font=\bfseries\large] at (1.4, -1.4) {$i^+$};
\end{tikzpicture}
\caption{Sistema in analisi: plasma confinato in geometria 1D \textit{slab} tra due pareti piane parallele a $x = -L/2$ e $x = L/2$ soggette a una differenza di potenziale $\Delta V$.}
\end{figure}

Le ipotesi fisiche del modello sono:
\begin{enumerate}
    \item \textbf{Confinamento 1D slab:} Plasma racchiuso tra due pareti parallele infinite in $x = -L/2$ e $x = +L/2$.
    \item \textbf{Bilancio globale di carica:} $n_e = n_i \implies$ nessun flusso netto di corrente macroscopica ($j = 0$).
    \item \textbf{Assenza di campo magnetico:} $B = 0$ (trascurabile).
    \item \textbf{Piccolo potenziale applicato alle pareti:}
    \begin{equation}
        \underbrace{e\Phi}_{\substack{\text{Electrostatic}\\\text{energy}}} \ll \underbrace{k_B T_e}_{\substack{\text{Thermal}\\\text{energy}}}
    \end{equation}
    \item \textbf{Condizione stazionaria (\textit{steady-state}):} $\dfrac{\partial}{\partial t} = 0$.
\end{enumerate}

\noindent\textbf{Obiettivi principali della derivazione:}
\begin{enumerate}
    \item Definire il profilo spaziale delle densit\`a $n_e(x)$ e $n_i(x)$ in presenza di un campo elettrostatico statico $\mathbf{E}$.
    \item Definire il profilo spaziale del potenziale elettrostatico $\Phi(x)$, legato al campo elettrico da $\mathbf{E} = -\nabla \Phi$.
\end{enumerate}

\subsection{Mathematical Derivation of $\lambda_D$ and $\Phi(x)$}

\paragraph{1$^\circ$ Set di Ipotesi:}
Il plasma \`e descritto come un sistema a due fluidi ($e^-$ e $i^+$), assumendo $Kn \ll 0.01$ per entrambe le specie. Scriviamo l'equazione di conservazione della quantit\`a di moto per un fluido generico:
\begin{equation}
    \rho \left( \frac{\partial v}{\partial t} + v \frac{\partial v}{\partial x} \right) = -\nabla p + \rho g
\end{equation}

\paragraph{2$^\circ$ Set di Ipotesi:}
\begin{itemize}
    \item Consideriamo separatamente i due fluidi (elettroni e ioni) con densit\`a di massa $\rho_\alpha = m_\alpha n_\alpha$.
    \item La forza peso $\rho g$ \`e trascurabile.
    \item Il sistema risente della forza elettrostatica per unit\`a di volume $F_E = q n E$ ($-e n_e E$ per gli elettroni, $+e n_i E$ per gli ioni).
\end{itemize}
Otteniamo cos\`i il sistema accoppiato per i due fluidi:
\begin{equation}
    \begin{cases}
        m_e n_e \left( \dfrac{\partial v_e}{\partial t} + v_e \dfrac{\partial v_e}{\partial x} \right) = -\nabla p_e - e n_e E \\[1.0em]
        m_i n_i \left( \dfrac{\partial v_i}{\partial t} + v_i \dfrac{\partial v_i}{\partial x} \right) = -\nabla p_i + e n_i E
    \end{cases}
\end{equation}

\paragraph{3$^\circ$ Set di Ipotesi e Semplificazioni:}
\begin{itemize}
    \item \textbf{Stazionariet\`a (\textit{steady-state}):} $\dfrac{\partial v_e}{\partial t} = 0$, $\dfrac{\partial v_i}{\partial t} = 0$.
    \item \textbf{Fluidi macroscopicamente fermi:} $v_e = v_i = 0$ (poich\'e $j = 0$).
    \item \textbf{Legge dei gas perfetti isotermi:} $p_e = n_e T_e$ e $p_i = n_i T_i$ (con la costante di Boltzmann $k_B$ inclusa nella definizione energetica della temperatura $T$, e $T_e \approx T_i = \text{costante}$).
    \item \textbf{Campo elettrostatico conservativo 1D:} $E = -\dfrac{\partial \Phi}{\partial x}$.
\end{itemize}
Applicando le semplificazioni al sistema di conservazione della quantit\`a di moto:
\begin{equation}
    \begin{cases}
        \cancelto{0}{m_e n_e \left( \cancel{\dfrac{\partial v_e}{\partial t}} + \cancel{v_e \dfrac{\partial v_e}{\partial x}} \right)} = -\nabla p_e - e n_e E \\[1.2em]
        \cancelto{0}{m_i n_i \left( \cancel{\dfrac{\partial v_i}{\partial t}} + \cancel{v_i \dfrac{\partial v_i}{\partial x}} \right)} = -\nabla p_i + e n_i E
    \end{cases}
    \quad \Longrightarrow \quad
    \begin{cases}
        \nabla p_e + e n_e E = 0 \\[0.6em]
        \nabla p_i - e n_i E = 0
    \end{cases}
\end{equation}
Esplicitando le derivate lungo $x$ e sostituendo $E = -\dfrac{\partial \Phi}{\partial x}$:
\begin{equation}
    \begin{cases}
        \dfrac{\partial p_e}{\partial x} - e \dfrac{\partial \Phi}{\partial x} n_e = 0 \\[0.8em]
        \dfrac{\partial p_i}{\partial x} + e \dfrac{\partial \Phi}{\partial x} n_i = 0
    \end{cases}
\end{equation}
Sviluppiamo l'equazione per gli elettroni sostituendo $p_e = n_e T_e$ (con $T_e$ uniforme):
\begin{equation}
    T_e \frac{\partial n_e}{\partial x} - e \frac{\partial \Phi}{\partial x} n_e = 0 \implies \int \frac{\partial \Phi}{\partial x}\, dx = \frac{T_e}{e} \int \frac{\partial n_e}{\partial x} \left[ \frac{1}{n_e} \right] dx
\end{equation}
Riconoscendo la derivata logaritmica $\frac{1}{n_e}\frac{\partial n_e}{\partial x} = \frac{\partial}{\partial x}\ln(n_e)$, otteniamo:
\begin{equation}
    \int \frac{\partial \Phi}{\partial x}\, dx = \frac{T_e}{e} \int \frac{\partial}{\partial x}\big(\ln(n_e)\big)\, dx \quad \longrightarrow \quad \boxed{\Phi(x) = \frac{T_e}{e} \ln(n_e) + k}
\end{equation}

\paragraph{Condizioni al Contorno e Relazioni di Boltzmann:}
Per chiudere il sistema e determinare la costante di integrazione $k$, imponiamo le condizioni al centro del plasma ($x = 0$), dove per simmetria il potenziale \`e nullo e la densit\`a \`e pari al valore indisturbato $n_0$:
\begin{equation}
    \begin{cases}
        \Phi(x) = \dfrac{T_e}{e} \ln(n_e) + k \\
        \Phi(x = 0) = 0 \\
        n_e(x = 0) = n_0
    \end{cases}
    \implies 0 = \frac{T_e}{e}\ln(n_0) + k \implies k = -\frac{T_e}{e}\ln(n_0)
\end{equation}
Sostituendo $k$ nell'espressione del potenziale:
\begin{equation}
    \Phi(x) = \frac{T_e}{e}\ln(n_e) - \frac{T_e}{e}\ln(n_0) = \frac{T_e}{e}\ln\left(\frac{n_e}{n_0}\right) \implies \ln\left(\frac{n_e}{n_0}\right) = \frac{e\Phi}{T_e}
\end{equation}
Invertendo il logaritmo per gli elettroni e procedendo in modo del tutto analogo per gli ioni (che hanno carica opposta $+e$), otteniamo le \textbf{relazioni di Boltzmann}:
\begin{equation}
    \boxed{
    \begin{cases}
        n_e(x) = n_0 e^{e\Phi(x) / T_e} \\[0.6em]
        n_i(x) = n_0 e^{-e\Phi(x) / T_i}
    \end{cases}
    }
\end{equation}
\textit{Osservazione fisica:} Laddove \`e presente un campo elettrico $E \neq 0$ ($\Phi \neq 0$), ne consegue che $n_e, n_i \neq n_0$ e inoltre $n_e \neq n_i$.

\paragraph{Equazione di Poisson e Lunghezza di Debye $\lambda_D$:}
Dalla legge di Gauss $\nabla \cdot \mathbf{E} = \dfrac{\rho}{\varepsilon_0}$ con densit\`a di carica $\rho = e(n_i - n_e)$ e $E = -\dfrac{\partial \Phi}{\partial x}$, scriviamo l'\textbf{equazione di Poisson} 1D:
\begin{equation}
    \frac{\partial^2 \Phi}{\partial x^2} = \frac{e}{\varepsilon_0}\big(n_e(x) - n_i(x)\big) = \frac{e}{\varepsilon_0} n_0 \left( e^{e\Phi / T_e} - e^{-e\Phi / T_i} \right)
\end{equation}
Ricordando l'ipotesi di piccolo potenziale applicato ($e\Phi \ll k_B T_e$), sviluppiamo gli esponenziali in serie di Taylor al primo ordine ($e^{\pm y} \approx 1 \pm y$):
\begin{equation}
    \frac{\partial^2 \Phi}{\partial x^2} = \frac{e}{\varepsilon_0} n_0 \left( \cancel{1} + \frac{e\Phi}{T_e} - \cancel{1} + \frac{e\Phi}{T_i} \right) = \left( \underbrace{\frac{e^2 n_0}{\varepsilon_0 T_e}}_{\frac{1}{\lambda_{De}^2}} + \underbrace{\frac{e^2 n_0}{\varepsilon_0 T_i}}_{\frac{1}{\lambda_{Di}^2}} \right) \Phi
\end{equation}
Definiamo la \textbf{lunghezza di Debye complessiva} $\lambda_D$ tramite:
\begin{equation}
    \frac{1}{\lambda_D^2} = \frac{1}{\lambda_{De}^2} + \frac{1}{\lambda_{Di}^2}, \qquad \text{dove} \quad \lambda_{De} = \left( \frac{\varepsilon_0 T_e}{e^2 n_0} \right)^{1/2}, \quad \lambda_{Di} = \left( \frac{\varepsilon_0 T_i}{e^2 n_0} \right)^{1/2}
\end{equation}
L'equazione di Poisson linearizzata assume la forma:
\begin{equation}
    \frac{\partial^2 \Phi}{\partial x^2} = \frac{1}{\lambda_D^2}\Phi \quad \Longrightarrow \quad \boxed{\Phi(x) = \frac{\Phi_0}{2} \frac{\sinh\left(\dfrac{x}{\lambda_D}\right)}{\sinh\left(\dfrac{L}{2\lambda_D}\right)}}
\end{equation}

\begin{figure}[htbp]
\centering
\begin{tikzpicture}[>=Stealth, scale=0.95, every node/.style={transform shape}]
    % Walls
    \draw[very thick] (-4, -2.0) -- (-4, 2.0);
    \draw[very thick] (4, -2.0) -- (4, 2.0);
    \foreach \y in {-1.5, -0.9, -0.3, 0.3, 0.9, 1.5} {
        \draw[thick] (-4, \y) -- (-4.5, \y-0.3);
        \node[red!80!black, font=\bfseries] at (-4.8, \y) {$+$};
        \draw[thick] (4, \y) -- (4.5, \y-0.3);
        \node[red!80!black, font=\bfseries] at (4.8, \y) {$-$};
    }
    \node[font=\bfseries] at (-4, -2.4) {$-L/2$};
    \node[font=\bfseries] at (4, -2.4) {$L/2$};

    % Axis
    \draw[thick] (-4, 0) -- (4, 0);
    \draw[very thick] (0, -0.2) -- (0, 0.2);
    \node[above, font=\bfseries\large] at (0, 0.35) {$\Phi = 0$};

    % Potential curve
    \draw[blue!75!black, ultra thick, domain=-4:4, samples=120]
        plot (\x, {-1.4 * sinh(\x/0.55) / sinh(4/0.55)});

    % Debye length annotations
    \draw[<->, violet!80!black, very thick] (-4, -0.5) -- (-2.9, -0.5);
    \node[violet!80!black, font=\bfseries\small, align=center] at (-3.45, -1.05) {$\lambda_D$\\[-2pt]\tiny DEBYE LENGTH};

    \draw[<->, violet!80!black, very thick] (2.9, 0.5) -- (4, 0.5);
    \node[violet!80!black, font=\bfseries\small] at (3.45, 0.85) {$\lambda_D$};
\end{tikzpicture}
\caption{Andamento spaziale del potenziale elettrostatico $\Phi(x)$ tra le pareti $-L/2$ e $L/2$: la variazione di potenziale \`e confinata in uno strato sottile di spessore $\lambda_D$ (\textit{sheath}) vicino alle pareti, mentre nel centro del plasma (\textit{bulk}) si ha $\Phi = 0$.}
\end{figure}

\subsection{Physical Meaning of Debye Shielding and Numerical Example}

\begin{figure}[htbp]
\centering
\begin{tikzpicture}[>=Stealth, scale=0.95, every node/.style={transform shape}]
    % Title legend
    \node[blue!80!black, font=\bfseries\large] at (-0.8, 2.3) {$n_e$};
    \node[orange!80!black, font=\bfseries\large] at (0.8, 2.3) {$n_i$};

    % Walls
    \draw[very thick] (-4, -2.0) -- (-4, 2.0);
    \draw[very thick] (4, -2.0) -- (4, 2.0);
    \foreach \y in {-1.5, -0.9, -0.3, 0.3, 0.9, 1.5} {
        \draw[thick] (-4, \y) -- (-4.5, \y-0.3);
        \node[red!80!black, font=\bfseries] at (-4.8, \y) {$+$};
        \draw[thick] (4, \y) -- (4.5, \y-0.3);
        \node[red!80!black, font=\bfseries] at (4.8, \y) {$-$};
    }
    \node[font=\bfseries] at (-4, -2.4) {$-L/2$};
    \node[font=\bfseries] at (4, -2.4) {$L/2$};

    % Axis (n_0)
    \draw[thick] (-4, 0) -- (4, 0);
    \draw[very thick] (0, -0.2) -- (0, 0.2);

    % Highlighted bulk region
    \fill[green!30, opacity=0.6, rounded corners=3pt] (-2.6, -0.18) rectangle (2.6, 0.18);
    \node[fill=green!25, rounded corners=4pt, inner sep=3pt, font=\bfseries\large] at (0, 0.85) {$\Phi = 0$};

    % n_e curve (blue: higher at positive wall -L/2, lower at negative wall +L/2)
    \draw[blue!80!black, ultra thick, domain=-4:4, samples=120]
        plot (\x, {-1.35 * sinh(\x/0.55) / sinh(4/0.55)});

    % n_i curve (orange/brown: lower at positive wall -L/2, higher at negative wall +L/2)
    \draw[orange!80!black, ultra thick, domain=-4:4, samples=120]
        plot (\x, {1.35 * sinh(\x/0.55) / sinh(4/0.55)});

    % Debye length markers
    \draw[<->, violet!80!black, very thick] (-4, -0.45) -- (-2.9, -0.45);
    \node[violet!80!black, font=\bfseries\small, align=center] at (-3.35, -1.1) {$\lambda_D$\\[-2pt]\tiny DEBYE LENGTH};

    \draw[<->, violet!80!black, very thick] (2.9, 0.45) -- (4, 0.45);
    \node[violet!80!black, font=\bfseries\small] at (3.45, 0.8) {$\lambda_D$};
\end{tikzpicture}
\caption{Andamento spaziale delle densit\`a $n_e(x)$ (blu) e $n_i(x)$ (arancione): lo sbilanciamento di carica \`e localizzato esclusivamente nelle guaine (\textit{sheath edge}) di spessore $\lambda_D$, mentre nel \textit{bulk} centrale vale $n_e = n_i = n_0$.}
\end{figure}

Il \textbf{Debye shielding effect} \`e uno dei criteri fondamentali per distinguere un semplice fluido o gas ionizzato da un vero plasma:
\begin{itemize}
    \item $\lambda_D$ rappresenta la \textbf{scala spaziale caratteristica} lungo la quale le forze e i potenziali elettrostatici decadono esponenzialmente.
    \item \textbf{Meccanismo fisico della schermatura ($*$):} Le cariche nel plasma sono libere di muoversi. Appena si applica il potenziale sulle pareti, gli elettroni (negativi) vengono subito attratti verso la parete positiva, mentre gli ioni (positivi) vengono respinti (e viceversa sulla parete opposta). Si crea cos\`i un sottile ``cuscinetto'' di cariche sbilanciate (chiamato \textit{sheath} o guaina) incollato alle pareti, di spessore $\lambda_D$ (non nullo a causa dell'agitazione termica delle particelle calde).
    \item \textbf{Annullamento del campo nel bulk ($**$):} Questo strato di carica genera un campo elettrico interno opposto che annulla esattamente quello imposto dalle pareti. Di conseguenza, nel centro del sistema (\textit{bulk plasma}), il potenziale $\Phi$ crolla a zero: il cuore del plasma \`e ``cieco'' al campo elettrico esterno e rimane perfettamente neutro ($n_e = n_i = n_0$).
    \item \textbf{Condizione geometrica di esistenza del plasma:} Affinch\'e esista una zona centrale neutra e schermata (\textit{bulk}), la dimensione caratteristica $L$ del contenitore deve essere molto maggiore dello spessore delle guaine alle pareti:
    \begin{equation}
        \boxed{L \gg \lambda_D}
    \end{equation}
    Se il contenitore fosse troppo piccolo ($L \lesssim \lambda_D$), i due strati di carica sulle pareti si sovrapporrebbero al centro: il campo elettrico esterno penetrerebbe ovunque e si avrebbe un semplice gas ionizzato in balia dei campi esterni, non un vero plasma.
\end{itemize}

\paragraph{Esempio Numerico in un Reattore a Fusione (Tokamak):}
Consideriamo i parametri tipici di un plasma da fusione in un Tokamak:
\begin{equation}
    T_e = 15\text{ keV}, \qquad n_0 = 2 \cdot 10^{20}\text{ part/m}^3
\end{equation}
Calcoliamo la lunghezza di Debye elettronica $\lambda_{De}$ (esprimendo $T_e$ in $\text{keV}$ nella formula pratica):
\begin{equation}
    \lambda_{De} = \left( \frac{\varepsilon_0 T_e}{e^2 n_0} \right)^{1/2} \approx 2.35 \cdot 10^5 \left( \frac{T_e\text{ [keV]}}{n_0\text{ [m}^{-3}\text{]}} \right)^{1/2} = 6.4 \cdot 10^{-5}\text{ m} \quad \Longrightarrow \quad \lambda_{De} \ll L
\end{equation}
Poich\'e in un Tokamak $L \sim 1\text{--}2\text{ m}$, la condizione $\lambda_{De} \ll L$ \`e ampiamente verificata.

% ============================================================================
\section{Quasi-Neutrality as a Consequence of Debye Shielding}
% ============================================================================

\subsection{Behavior of the Bulk Plasma Under Potential Fluctuations}
Richiamando i profili di densit\`a di Boltzmann:
\begin{equation}
    n_e = n_0 e^{e\Phi / T_e}, \qquad n_i = n_0 e^{-e\Phi / T_i}
\end{equation}
abbiamo osservato che:
\begin{itemize}
    \item Dove $\Phi = 0$ (nella zona centrale del \textit{bulk}), vale $n_e = n_i = n_0$.
    \item Nei bordi (zona di spessore $\lambda_D$, \textit{sheath edge}), si ha $n_e \neq n_i$.
\end{itemize}
\textbf{Domanda fisica:} \textit{Cosa succede nel bulk se \`e presente una differenza di potenziale $\Delta \Phi \neq 0$ (ovvero un campo elettrostatico $\mathbf{E}$ generato da fluttuazioni spontanee)?} Mostriamo matematicamente che anche in questo caso vale la condizione di \textbf{quasi-neutralit\`a} $n_e \approx n_i$.

\begin{figure}[htbp]
\centering
\begin{tikzpicture}[>=Stealth, scale=0.95, every node/.style={transform shape}]
    % Left: Charge blob with internal E field
    \draw[red!80!black, very thick, fill=red!5] (-3.5, 0) ellipse (1.8cm and 1.2cm);
    \node[red!80!black, font=\bfseries\small] at (-3.5, 1.5) {CARICHE (Fluttuazioni nel Bulk)};
    \node[blue!80!black, font=\bfseries\large] at (-4.3, 0.3) {$n_e$};
    \node[orange!80!black, font=\bfseries\large] at (-2.7, -0.3) {$n_i$};
    \draw[->, green!60!black, ultra thick] (-2.2, 0.2) -- (-0.8, 0.2) node[midway, above, font=\bfseries] {$\vec{E}$};

    % Right: Scale estimation diagram
    \draw[thick] (2.0, -1.2) -- (2.0, 1.2);
    \draw[thick] (6.0, -1.2) -- (6.0, 1.2);
    \draw[dashed] (2.0, 0) -- (6.0, 0);
    \node[font=\small\bfseries] at (2.0, -1.5) {$-L/2$};
    \node[font=\small\bfseries] at (6.0, -1.5) {$L/2$};
    \node[font=\small] at (4.0, 0.35) {$\Phi \sim \text{scala } L$};
    \node[font=\small, align=center] at (4.0, -0.55) {$E = -\nabla\Phi \sim \dfrac{\Phi}{L}$};
\end{tikzpicture}
\caption{Schematizzazione di una fluttuazione spontanea di carica nel \textit{bulk} ($e\Phi \simeq k_B T_e$) e stima degli ordini di grandezza dei gradienti spaziali sulla scala macroscopica $L$.}
\end{figure}

\subsection{Mathematical Derivation of the Density Perturbation $\delta \hat{n}$}
Consideriamo il plasma in regime stazionario con $T_e \simeq \text{cost} \neq 0$ (l'agitazione termica fa s\`i che $v_{e^-} \gg v_{i^+}$, generando possibili separazioni locali di carica $n_e \neq n_i$ e quindi $\Delta \Phi \neq 0$). Rimuoviamo l'ipotesi di piccolo potenziale ($\xcancel{e\Phi \ll k_B T_e}$) e consideriamo fluttuazioni spontanee fino all'ordine dell'energia termica:
\begin{equation}
    e\Phi \simeq k_B T_e
\end{equation}
Introduciamo due variabili adimensionali (normalizzate):
\begin{enumerate}
    \item \textbf{Potenziale normalizzato (\textit{Normalised potential}):}
    \begin{equation}
        \hat{\Phi} = \frac{e\Phi}{T_e} = \frac{\text{Electrostatic energy}}{\text{Thermal energy}}
    \end{equation}
    \item \textbf{Perturbazione di densit\`a normalizzata (\textit{Normalised density perturbation}):}
    \begin{equation}
        \delta \hat{n} = \frac{n_e - n_i}{n_0} \quad \text{(scostamento relativo dalla perfetta neutralit\`a)}
    \end{equation}
\end{enumerate}

Partiamo dall'\textbf{equazione di Poisson}:
\begin{equation}
    \nabla^2 \Phi = \frac{e}{\varepsilon_0}(n_e - n_i)
\end{equation}
Moltiplichiamo ambo i membri per $\dfrac{e}{T_e}$ (costante) e moltiplichiamo e dividiamo il membro destro per $n_0$:
\begin{align}
    \nabla^2 \left( \frac{e\Phi}{T_e} \right) &= \frac{e^2}{\varepsilon_0 T_e}(n_e - n_i) \frac{n_0}{n_0} \\[0.6em]
    \nabla^2 \hat{\Phi} &= \underbrace{\frac{e^2 n_0}{\varepsilon_0 T_e}}_{1/\lambda_D^2} \underbrace{\left( \frac{n_e - n_i}{n_0} \right)}_{\delta \hat{n}} \quad \Longrightarrow \quad \nabla^2 \hat{\Phi} = \frac{\delta \hat{n}}{\lambda_D^2}
\end{align}
Effettuiamo ora l'analisi agli ordini di grandezza su scala macroscopica $L$ (dove $\Phi$ varia sulla scala spaziale $L$ del sistema):
\begin{equation}
    \begin{cases}
        x \sim L \\
        \dfrac{d}{dx} \sim \dfrac{1}{L} \\
        \dfrac{d^2}{dx^2} \sim \dfrac{1}{L^2}
    \end{cases}
    \implies \nabla^2 \hat{\Phi} \sim \frac{\hat{\Phi}}{L^2}
\end{equation}
Uguagliando gli ordini di grandezza nella relazione di Poisson adimensionale:
\begin{equation}
    \frac{\hat{\Phi}}{L^2} \sim \frac{\delta \hat{n}}{\lambda_D^2} \quad \Longrightarrow \quad \boxed{\delta \hat{n} = \hat{\Phi} \left( \frac{\lambda_D}{L} \right)^2}
\end{equation}

\subsection{Order-of-Magnitude Evaluation and Physical Conclusions}
Non serve avere un potenziale rigorosamente nullo ($\Phi = 0$) per garantire la quasi-neutralit\`a ($n_e \approx n_i$). Infatti, in un plasma tipico:
\begin{equation}
    \lambda_D \ll L \implies \delta \hat{n} \ll \hat{\Phi}
\end{equation}
Inserendo i valori tipici di un reattore a fusione:
\begin{equation}
    \left.
    \begin{aligned}
        \lambda_D &\approx 10^{-5}\text{ m} \\
        L &\approx 1\text{ m} \\
        \hat{\Phi} &\approx 1 \quad (\text{potenziale forte che penetra, } e\Phi \approx T_e)
    \end{aligned}
    \right\}
    \implies \delta \hat{n} \approx \left( \frac{\lambda_D}{L} \right)^2 \approx 10^{-10} \neq 0!
\end{equation}

\noindent\textbf{Osservazioni fisiche fondamentali (dagli appunti):}
\begin{itemize}
    \item Anche in presenza di un potenziale forte ($\hat{\Phi} \approx 1$), il fattore geometrico quadratico $\left(\frac{\lambda_D}{L}\right)^2 \approx 10^{-10}$ abbatte completamente lo sbilanciamento di densit\`a nel \textit{bulk}, garantendo che la perturbazione di densit\`a sia del tutto trascurabile ($n_e \approx n_i$, \textbf{condizione di quasi-neutralit\`a}).
    \item Tuttavia, \`e essenziale notare che $\delta \hat{n} \approx 10^{-10} \mathbf{\neq 0}$: \textbf{se il plasma fosse esattamente neutro ($\delta \hat{n} = 0$ identicamente), non potrebbe generarsi il campo elettrico interno $\mathbf{E}_{\text{int}}$ e il campo elettrico esterno $\mathbf{E}_{\text{est}}$ non verrebbe annullato!} \`E proprio questo piccolissimo sbilanciamento relativo ($1$ parte su $10^{10}$) a sostenere la schermatura elettrostatica.
\end{itemize}

\subsection{Debye Shielding: Take-Home Messages}
\begin{enumerate}
    \item \textbf{Macro-Scale Quasi-Neutrality (\textit{Homogeneous Fluid}):} Dimostra che al di fuori di uno strato limite microscopico di spessore $\sim \lambda_D$ (\textit{sheath}), il \textit{bulk plasma} rimane privo di campo elettrico e quasi-neutro ($n_e \approx n_i$).
    \item \textbf{Establishes the Fluid Model Boundary:} Poich\'e il plasma \`e sostanzialmente neutro nella quasi totalit\`a del suo volume (\textit{bulk region}), ci\`o giustifica ulteriormente la trattazione macroscopica del plasma come un fluido omogeneo.
    \item \textbf{Explains the Conductive Nature of Plasma:} Fornisce la prova fisica di come l'elevata conducibilit\`a elettrica permetta alle cariche libere di riarrangiarsi istantaneamente per schermare i campi elettrostatici esterni.
    \begin{quote}
        \textit{Sintesi manoscritta finale:} $\text{Conductivity} \implies$ libert\`a di movimento e riarrangiamento veloce delle cariche: non appena si applica un campo esterno $\vec{E}_{\text{esterno}}$, le cariche si ridispongono rapidamente ai bordi (un bordo carico $+$, l'altro carico $-$) creando un campo elettrico interno $\vec{E}_{\text{int}}$ che annulla $\vec{E}_{\text{est}}$ (\textit{Shield Electric Field}).
    \end{quote}
\end{enumerate}

\pagebreak

% ============================================================================
\section{Introductory Exercises on Debye Shielding and Sheath Electric Field}
% ============================================================================

\subsection{Exercise: Example 1 (Debye Length in a Discharge Plasma)}

\textbf{Problem Statement:} A low-temperature laboratory discharge plasma has an electron density of $n_e = 10^{16}\text{ m}^{-3}$ and an electron temperature of $T_e = 2\text{ eV}$. Assume $T_e \approx T_i$. Calculate the corresponding Debye length $\lambda_D$.

\paragraph{Physical Constants and Conversion:}
\begin{itemize}
    \item $\varepsilon_0$: vacuum permittivity $[\text{F/m}] \implies \varepsilon_0 = 8.85 \cdot 10^{-12}\text{ F/m}$
    \item $e$: elementary charge $[\text{C}] \implies e = 1.602 \cdot 10^{-19}\text{ C}$
    \item Conversion of temperature into energy units $[\text{J}]$:
    \begin{equation}
        T_e\,[\text{J}] = 2\text{ eV} \cdot 1.602 \cdot 10^{-19}\text{ J/eV}
    \end{equation}
\end{itemize}

\paragraph{Solution:}
Starting from the definition of the electron Debye length $\lambda_D$ (with unperturbed bulk density $n_0 = n_e$):
\begin{equation}
    \frac{1}{\lambda_D^2} = \frac{e^2 n_0}{\varepsilon_0 T_e} \quad \longrightarrow \quad \lambda_D = \sqrt{\frac{\varepsilon_0 T_e}{e^2 n_0}}\quad [\text{m}]
\end{equation}
Substituting the numerical values and simplifying the conversion factor $1.602 \cdot 10^{-19}$ against the squared elementary charge in the denominator:
\begin{equation}
    \lambda_D = \sqrt{\frac{8.85 \cdot 10^{-12} \cdot 2 \cdot \cancel{1.602 \cdot 10^{-19}}}{\left(1.602 \cdot 10^{-19}\right)^{\cancel{2}} \cdot 10^{16}}} \simeq 1.05 \cdot 10^{-4}\text{ m} = 0.105\text{ mm}
\end{equation}

\subsection{Exercise: Example 2 (Sheath Electric Field Estimation)}

\textbf{Problem Statement:} A laboratory discharge plasma has an electron density of $n_e = 10^{16}\text{ m}^{-3}$ and an electron temperature of $T_e = \underline{10\text{ eV}}$. Suppose a potential difference of $V_0 = 100\text{ V}$ is applied between the bulk plasma and a metal wall.
\begin{enumerate}
    \item Calculate the Debye length ($\lambda_D$).
    \item Estimate the order of magnitude of the maximum electric field $E$ that develops within the sheath, assuming the entire potential drop occurs over a thickness roughly equal to one Debye length ($\lambda_D$).
\end{enumerate}

\paragraph{Solution:}
\begin{enumerate}
    \item \textbf{Debye length calculation:}
    \begin{equation}
        \lambda_D = \sqrt{\frac{\varepsilon_0 T_e}{e^2 n_0}} = \sqrt{\frac{8.85 \cdot 10^{-12} \cdot 10 \cdot \cancel{1.602 \cdot 10^{-19}}}{\left(1.602 \cdot 10^{-19}\right)^{\cancel{2}} \cdot 10^{16}}} \simeq 0.235\text{ mm}
    \end{equation}
    \item \textbf{Maximum electric field in the sheath:}
    From electrostatics, reducing the gradient from 3D to a 1D spatial coordinate $x$ perpendicular to the wall:
    \begin{equation}
        \vec{E} = -\nabla \Phi \quad \xrightarrow{\text{1D}} \quad E = -\frac{d\Phi}{dx}
    \end{equation}
    Approximating the differentials over the sheath thickness:
    \begin{equation}
        \begin{cases}
            d\Phi \sim \Delta V = V_0 = 100\text{ V} \\
            dx \sim \lambda_D \simeq 0.235 \cdot 10^{-3}\text{ m}
        \end{cases}
    \end{equation}
    The magnitude of the electric field is therefore:
    \begin{equation}
        |E| = \left|-\frac{\Delta V}{\lambda_D}\right| = \frac{100}{0.235 \cdot 10^{-3}} \simeq 425\text{ kV/m}
    \end{equation}
    \emph{Physical observation:} This electric field is \textbf{very high}, but it is localized exclusively within the thin Debye sheath of thickness $\sim \lambda_D$ near the wall and \textbf{not} in the quasi-neutral bulk plasma.
\end{enumerate}

% ============================================================================
\section{Plasma Frequency and Debye Shielding with an AC Field}
% ============================================================================

\subsection{Problem Formulation: DC vs.\ AC Applied Fields}

So far, we have considered what happens to a plasma when an electrostatic field is applied. This analysis holds when we simply have a DC (Direct Current) field.

What happens if we apply an AC (Alternating Current) field to the parallel infinite walls? We want to study the behavior of our system when we apply a potential difference $\Delta V(t)$ that varies in time (representing a \emph{small perturbation} of the electric field $E$), and determine whether the plasma is still capable of shielding the AC field out. This leads to the definition of the \textbf{plasma frequency} $\omega_p$.

\begin{figure}[htbp]
\centering
\begin{tikzpicture}[>=Stealth, scale=1.0]
    % Left Wall
    \draw[thick] (0, 0) -- (0, 3.5);
    \foreach \y in {0.2, 0.7, 1.2, 1.7, 2.2, 2.7, 3.2} {
        \draw[thin] (-0.3, \y-0.2) -- (0, \y);
    }
    % Right Wall
    \draw[thick] (4.5, 0) -- (4.5, 3.5);
    \foreach \y in {0.2, 0.7, 1.2, 1.7, 2.2, 2.7, 3.2} {
        \draw[thin] (4.5, \y) -- (4.8, \y-0.2);
    }

    % Positive charges on left wall
    \node[red!80!black, font=\bfseries] at (0.25, 3.0) {$+$};
    \node[red!80!black, font=\bfseries] at (0.25, 2.3) {$+$};
    \node[red!80!black, font=\bfseries] at (0.25, 1.6) {$+$};
    \node[red!80!black, font=\bfseries] at (0.25, 0.9) {$+$};

    % Plasma particles inside
    \node[blue!80!black] at (0.8, 2.8) {$e^-$};
    \node[blue!80!black] at (0.9, 1.9) {$e^-$};
    \node[blue!80!black] at (0.75, 1.1) {$e^-$};
    \node[red!80!black] at (1.5, 2.4) {$i^+$};
    \node[red!80!black] at (1.7, 1.2) {$i^+$};
    \node[blue!80!black] at (2.3, 2.0) {$e^-$};
    \node[red!80!black] at (2.8, 2.6) {$i^+$};

    % External circuit with AC source
    \draw[thick] (0, 0) -- (0, -0.8) -- (1.85, -0.8);
    \draw[thick] (4.5, 0) -- (4.5, -0.8) -- (2.65, -0.8);
    \draw[thick] (2.25, -0.8) circle (0.4);
    % Sine wave inside AC source
    \draw[thick, domain=-0.25:0.25, smooth, variable=\x] plot ({2.25+\x}, {-0.8 + 0.18*sin(\x*720)});
    \node[below=0.45cm] at (2.25, -0.8) {$\Delta V(t)$};

    % Annotation on the right
    \node[align=left, anchor=west, font=\small] at (5.5, 1.7) {
        \textbf{Goal:} Observe the behavior\\
        of the system under a time-varying\\
        potential $\Delta V(t)$:\\
        \emph{small perturbation of $\vec{E}(x,t)$}
    };
\end{tikzpicture}
\caption{Schematic of a plasma confined between two parallel infinite conductive walls connected to an alternating voltage source $\Delta V(t)$.}
\end{figure}

\subsection{Two-Fluid Physical Mechanism and Characteristic Time Scales}

To answer this question, let us first recall how a plasma shields an electric field:
\begin{itemize}
    \item $n_e$ and $n_i$ redistribute in such a way that they generate a (small) opposing electric field $E$, which compensates the externally applied $E$ and shields it completely out for distances $> \lambda_D$.
    \item This effect is generated by the motion of both ions and electrons (\textbf{2-fluid description}).
    \item However, ions and electrons exhibit very different dynamical behaviors, primarily due to their vastly different masses ($m_i \gg m_e$).
\end{itemize}

Let us give a quantitative description of this different behavior in **standard tokamaks**:
\begin{itemize}
    \item Debye length: $\lambda_D \approx 5.3 \cdot 10^{-5}\text{ [m]}$
    \item Temperatures: $T_e \approx T_i = 10\text{ [keV]}$
    \item Particle masses (considering a Deuterium plasma, $\text{D}^+$):
    \begin{equation}
        m_e = 9.109 \cdot 10^{-31}\text{ kg}, \qquad m_{\text{ion}} = m_{\text{D}^+} = 3.34 \cdot 10^{-27}\text{ kg}
    \end{equation}
\end{itemize}

Introducing the **thermal speed** $v_{\text{th}} = \sqrt{\frac{T}{m}}$ for each species:
\begin{align}
    v_e &= \sqrt{\frac{T_e}{m_e}} \simeq 42 \cdot 10^3\text{ km/s} = 4.2 \cdot 10^7\text{ m/s} \\
    v_i &= \sqrt{\frac{T_i}{m_i}} = \sqrt{\frac{T_e}{m_i}} \simeq 980\text{ km/s} \quad (\sim 900\text{ km/s})
\end{align}
The characteristic time required for each species to cover a distance equal to $\lambda_D$ is:
\begin{align}
    \tau_e &= \frac{\lambda_D}{v_e} \approx 1.3 \cdot 10^{-12}\text{ [s]} \\
    \tau_i &= \frac{\lambda_D}{v_i} \approx 1.8 \cdot 10^{-9}\text{ [s]}
\end{align}
\textbf{Conclusion:} Since $\tau_e \ll \tau_i$, the redistribution of electrons is much quicker than that of ions. Electrons are roughly three orders of magnitude faster than ions in covering the distance $\lambda_D$ and rearranging their spatial distribution.

\subsection{Classification of AC Perturbation Regimes}

Let $\tau_{AC} = \frac{1}{\omega}$ be the characteristic period of the applied AC perturbation of angular frequency $\omega$. Depending on the frequency of the applied AC field, three distinct behaviors develop in the plasma:

\begin{enumerate}
    \item \textbf{Very slow AC potential variation ($\tau_{AC} \gg 10^{-9}\text{ [s]}$, i.e., $\tau_{AC} \gg \tau_i > \tau_e$):}\\
    Both ions and electrons have plenty of time to rearrange themselves and shield the AC field.\\
    \emph{Note:} This regime practically never happens in fusion reactors for high-frequency waves.

    \item \textbf{Very fast AC potential variation ($\tau_{AC} \ll 10^{-12}\text{ [s]}$, i.e., $\tau_{AC} \ll \tau_e < \tau_i$):}\\
    Neither ions nor electrons manage to rearrange before another variation of the AC field occurs $\longrightarrow$ \textbf{no electric field is shielded anymore}.\\
    \emph{Note:} This regime can happen in reactors (e.g., high-frequency electromagnetic wave injection).

    \item \textbf{Intermediate range ($10^{-12}\text{ [s]} \ll \tau_{AC} \ll 10^{-9}\text{ [s]}$, i.e., $\tau_e \ll \tau_{AC} \ll \tau_i$):}\\
    Electrons have time to rearrange before a variation of the AC field occurs, whereas ions do not $\longrightarrow$ \textbf{partial shielding of the AC field, performed entirely by electrons}.\\
    \emph{Note:} This regime usually happens in reactors and is the case we study in detail below:
    \begin{equation}
        \boxed{\text{Key Physical Consideration: Ions are FROZEN } (v_i = 0),\text{ while ELECTRONS MOVE } (v_e \neq 0).}
    \end{equation}
\end{enumerate}

% ============================================================================
\section{Mathematical Derivation of the Plasma Frequency $\omega_p$}
% ============================================================================

\subsection{Governing Two-Fluid Equations and Simplifying Assumptions}

\textbf{Main Aim:} Define the spatial and temporal shape of the electric potential $\Phi(x,t)$ developed in the system when an AC field is applied.

\begin{figure}[htbp]
\centering
\begin{tikzpicture}[>=Stealth, scale=0.95]
    % Left and Right Walls
    \draw[thick] (0, 0) -- (0, 3.2);
    \draw[thick] (6, 0) -- (6, 3.2);

    % Centerline reference
    \draw[dashed, gray] (0, 1.6) -- (6, 1.6);

    % Instantaneous potential curve \Phi(t)
    \draw[thick, blue!80!black] (0, 2.9) .. controls (0.4, 1.7) and (0.9, 1.6) .. (1.5, 1.6)
        -- (4.5, 1.6) .. controls (5.1, 1.6) and (5.6, 1.5) .. (6, 0.3);

    \node[blue!80!black, above] at (3.0, 1.65) {$\Phi(x,t)$};
    \node[blue!80!black] at (0.5, 2.5) {$e^-$};
    \node[red!80!black] at (5.5, 2.2) {$i^+$};

    % External AC circuit
    \draw[thick] (0, 0) -- (0, -0.7) -- (2.6, -0.7);
    \draw[thick] (6, 0) -- (6, -0.7) -- (3.4, -0.7);
    \draw[thick] (3.0, -0.7) circle (0.4);
    \draw[thick, domain=-0.25:0.25, smooth, variable=\x] plot ({3.0+\x}, {-0.7 + 0.18*sin(\x*720)});
    \node[below=0.45cm] at (3.0, -0.7) {$\Delta V(t)$};
\end{tikzpicture}
\caption{Instantaneous potential profile $\Phi(x,t)$ established between the electrodes under an AC voltage $\Delta V(t)$.}
\end{figure}

We start the derivation from the 1D momentum conservation equations for both species ($e^-$ and $i^+$):
\begin{equation}
    \begin{cases}
        m_e n_e \left( \dfrac{\partial v_e}{\partial t} + v_e \dfrac{\partial v_e}{\partial x} \right) = -\dfrac{\partial p_e}{\partial x} - e n_e E \\[1.2em]
        m_i n_i \left( \dfrac{\partial v_i}{\partial t} + v_i \dfrac{\partial v_i}{\partial x} \right) = -\dfrac{\partial p_i}{\partial x} + e n_i E
    \end{cases}
\end{equation}

\paragraph{Physical Assumptions:}
\begin{enumerate}
    \item \textbf{Not steady-state:} $\dfrac{\partial}{\partial t} \neq 0$.
    \item \textbf{Electrons move with velocity $v_e$:} This generates an electron current.
    \item \textbf{Ions are frozen:} Because of their large inertia on the $\tau_{AC}$ timescale,
    \begin{equation}
        v_i = 0, \qquad \frac{\partial v_i}{\partial t} = 0, \qquad \frac{\partial n_i}{\partial t} = 0 \implies n_i = n_0 \quad (\text{uniform background density})
    \end{equation}
    \item \textbf{Applied time-varying voltage is small ($e\Phi \ll T_e$):} The generated electric field has a negligible inductive component ($\partial \vec{A}/\partial t \approx 0$):
    \begin{equation}
        E(x,t) = -\nabla \Phi(x,t) - \cancelto{0}{\frac{\partial A(x,t)}{\partial t}} = -\nabla \Phi(x,t) \quad \xrightarrow{\text{1D}} \quad E(x,t) = -\frac{\partial \Phi(x,t)}{\partial x}
    \end{equation}
\end{enumerate}

Applying the frozen-ion assumption cancels the ion momentum equation entirely:
\begin{equation}
    \begin{cases}
        m_e n_e \left( \dfrac{\partial v_e}{\partial t} + v_e \dfrac{\partial v_e}{\partial x} \right) = -\dfrac{\partial p_e}{\partial x} - e n_e E \\[1.2em]
        \xcancel{m_i n_i \left( \dfrac{\partial v_i}{\partial t} + v_i \dfrac{\partial v_i}{\partial x} \right) = -\dfrac{\partial p_i}{\partial x} + e n_i E} \qquad \text{(ions are frozen)}
    \end{cases}
\end{equation}
Retaining only the electron momentum equation and substituting the isothermal pressure relation $p_e = n_e T_e$ (with $T_e = \text{const}$) and $E = -\frac{\partial \Phi}{\partial x}$:
\begin{equation}
    m_e n_e \left( \frac{\partial v_e}{\partial t} + v_e \frac{\partial v_e}{\partial x} \right) = -T_e \frac{\partial n_e}{\partial x} + e n_e \frac{\partial \Phi}{\partial x}
\end{equation}

We have \textbf{three unknowns} ($n_e$, $v_e$, $\Phi$), so we need two additional equations to close the PDE system:
\begin{itemize}
    \item \textbf{Momentum equation:}
    \begin{equation}
        m_e n_e \left( \frac{\partial v_e}{\partial t} + v_e \frac{\partial v_e}{\partial x} \right) = -T_e \frac{\partial n_e}{\partial x} + e n_e \frac{\partial \Phi}{\partial x}
    \end{equation}
    \item \textbf{Continuity equation (for electrons):}
    \begin{equation}
        \frac{\partial n_e}{\partial t} + \frac{\partial}{\partial x}(n_e v_e) = 0
    \end{equation}
    \item \textbf{Poisson's equation:}
    \begin{equation}
        \frac{\partial^2 \Phi}{\partial x^2} = \frac{e}{\varepsilon_0}(n_e - n_i)
    \end{equation}
\end{itemize}
This forms a system of \textbf{3 non-linear coupled PDEs}, requiring perturbative linearization.

\subsection{Perturbation Theory and Linearization}

\paragraph{Harmonic Perturbation Expansion:}
We assume that the applied AC field has a sinusoidal shape in time with perturbation frequency $\omega$:
\begin{equation}
    \Phi(x,t) = \underbrace{\Phi_0(x)}_{\text{Order } 0} + \underbrace{\Phi_1(x) e^{-i\omega t}}_{\substack{\text{Small perturbation} \\ \text{(Order } 1\text{)}}}
\end{equation}
Assuming that all other plasma parameters follow the same harmonic variation in time:
\begin{align}
    v_e(x,t) &= v_{e,0} + v_{e,1}(x) e^{-i\omega t}, & v_{e,1} &\ll v_{e,0} \\
    n_e(x,t) &= n_{e,0} + n_{e,1}(x) e^{-i\omega t}, & n_{e,1} &\ll n_{e,0}
\end{align}

In general perturbation theory, a generic field quantity $Q(x,t)$ is expanded in powers of a smallness parameter $\varepsilon \ll 1$:
\begin{equation}
    Q(x,t) = \underbrace{Q_0(x)}_{\substack{\text{Unperturbed state:} \\ n_{e,0} = n_0 \\ v_{e,0} = 0 \\ \Phi_0 = 0}} + \underbrace{\varepsilon Q_1(x,t)}_{\substack{\text{1st-order perturbation:} \\ n_{e,1}(x)e^{-i\omega t} \\ v_{e,1}(x)e^{-i\omega t} \\ \Phi_1(x)e^{-i\omega t}}} + \cancelto{0}{\underbrace{\varepsilon^2 Q_2(x,t)}_{\substack{\text{2nd order} \\ \text{(negligible)}}}} + \cancelto{0}{\mathcal{O}(\varepsilon^3)}
\end{equation}
Separating the static bulk equilibrium from the perturbed terms ($Q(x,t) = Q_0(x) + Q_1(x,t)$), our solution system reads:
\begin{equation}
    \begin{cases}
        n_e(x,t) = n_0 + n_{e,1}(x) e^{-i\omega t} \\
        n_i(x,t) = n_0 + \cancelto{0}{n_{i,1}(x) e^{-i\omega t}} \\
        v_e(x,t) = \cancelto{0}{v_0} + v_{e,1}(x) e^{-i\omega t} \\
        \Phi(x,t) = \cancelto{0}{\Phi_0} + \Phi_1(x) e^{-i\omega t}
    \end{cases}
\end{equation}

\paragraph{Substitution into the Momentum Equation:}
Substituting the expanded variables into the electron momentum equation and cancelling all $2^\circ$-order terms (products of two $1^\circ$-order perturbations $\propto e^{-2i\omega t}$) as well as spatial derivatives of the uniform background density ($\partial n_0/\partial x = 0$):
\begin{multline}
    m_e \left(n_0 + \cancel{n_{e,1} e^{-i\omega t}}\right) \left( \frac{\partial \left(v_{e,1} e^{-i\omega t}\right)}{\partial t} + \cancel{v_{e,1} e^{-i\omega t} \frac{\partial \left(v_{e,1} e^{-i\omega t}\right)}{\partial x}} \right) = \\
    = -T_e \frac{\partial}{\partial x}\left(\cancel{n_0} + n_{e,1} e^{-i\omega t}\right) + e \frac{\partial \left(\Phi_1 e^{-i\omega t}\right)}{\partial x} \left(n_0 + \cancel{n_{e,1} e^{-i\omega t}}\right)
\end{multline}
Thus, the linearized $1^\circ$-order momentum equation becomes:
\begin{equation}
    m_e n_0 \frac{\partial \left(v_{e,1} e^{-i\omega t}\right)}{\partial t} = -T_e \frac{\partial \left(n_{e,1} e^{-i\omega t}\right)}{\partial x} + e n_0 \frac{\partial \Phi_1}{\partial x} e^{-i\omega t}
\end{equation}

\paragraph{Full Set of 3 Linearized PDEs (1st-Order Terms Only):}
\begin{itemize}
    \item \textbf{Momentum:}
    \begin{equation}
        m_e n_0 \frac{\partial \left(v_{e,1} e^{-i\omega t}\right)}{\partial t} = -T_e \frac{\partial \left(n_{e,1} e^{-i\omega t}\right)}{\partial x} + e n_0 \frac{\partial \left(\Phi_1 e^{-i\omega t}\right)}{\partial x}
    \end{equation}
    \item \textbf{Continuity:}
    \begin{equation}
        \frac{\partial \left(n_{e,1} e^{-i\omega t}\right)}{\partial t} + \frac{\partial \left(n_0 v_{e,1} e^{-i\omega t}\right)}{\partial x} = 0
    \end{equation}
    \item \textbf{Poisson:}
    \begin{equation}
        \frac{\partial^2 \left(\Phi_1 e^{-i\omega t}\right)}{\partial x^2} = \frac{e}{\varepsilon_0} \left( \left(\cancel{n_0} + n_{e,1} e^{-i\omega t}\right) - \cancel{n_0} \right) = \frac{e}{\varepsilon_0} n_{e,1} e^{-i\omega t}
    \end{equation}
\end{itemize}

\subsection{Reduction to a Single Differential Equation for $\Phi_1(x)$}

Replacing the time derivative operator with $\dfrac{\partial}{\partial t} = -i\omega$ and cancelling the common harmonic factor $e^{-i\omega t}$ across all three equations:
\begin{align}
    m_e n_0 \left(-i\omega v_{e,1} \cancel{e^{-i\omega t}}\right) &= -T_e \frac{\partial n_{e,1}}{\partial x} \cancel{e^{-i\omega t}} + e n_0 \frac{\partial \Phi_1}{\partial x} \cancel{e^{-i\omega t}} \\
    -i\omega n_{e,1}(x) \cancel{e^{-i\omega t}} + n_0 \frac{\partial v_{e,1}(x)}{\partial x} \cancel{e^{-i\omega t}} &= 0 \\
    \frac{\partial^2 \Phi_1}{\partial x^2} \cancel{e^{-i\omega t}} &= \frac{e}{\varepsilon_0} n_{e,1} \cancel{e^{-i\omega t}}
\end{align}

From the **continuity equation**, we solve for $\dfrac{\partial v_{e,1}}{\partial x}$:
\begin{equation}
    \boxed{\frac{\partial v_{e,1}}{\partial x} = i\omega \frac{n_{e,1}(x)}{n_0}}
\end{equation}
Next, we differentiate the **momentum equation** with respect to $x$:
\begin{equation}
    \frac{\partial}{\partial x} \left( -i\omega m_e n_0 v_{e,1} = -T_e \frac{\partial n_{e,1}}{\partial x} + e n_0 \frac{\partial \Phi_1}{\partial x} \right)
\end{equation}
Using the imaginary unit identity $\dfrac{1}{i} = -i$ (so that $\dfrac{1}{-i} = i$), we isolate $\dfrac{\partial v_{e,1}}{\partial x}$:
\begin{equation}
    \frac{\partial v_{e,1}}{\partial x} = \frac{1}{-i\omega m_e n_0} \left[ -T_e \frac{\partial^2 n_{e,1}}{\partial x^2} + e n_0 \frac{\partial^2 \Phi_1}{\partial x^2} \right] = \frac{i}{\omega m_e n_0} \left[ -T_e \frac{\partial^2 n_{e,1}}{\partial x^2} + e n_0 \frac{\partial^2 \Phi_1}{\partial x^2} \right]
\end{equation}

Equating the two expressions found for $\dfrac{\partial v_{e,1}}{\partial x}$:
\begin{equation}
    \cancel{i}\omega \frac{n_{e,1}}{\cancel{n_0}} = \frac{\cancel{i}}{\omega m_e \cancel{n_0}} \left[ -T_e \frac{\partial^2 n_{e,1}}{\partial x^2} + e n_0 \frac{\partial^2 \Phi_1}{\partial x^2} \right]
\end{equation}
Rearranging for $n_{e,1}(x)$:
\begin{equation}
    n_{e,1}(x) = \frac{e n_0}{\omega^2 m_e} \frac{\partial^2 \Phi_1}{\partial x^2} - \frac{T_e}{\omega^2 m_e} \frac{\partial^2 n_{e,1}}{\partial x^2}
\end{equation}

We now substitute this expression for $n_{e,1}(x)$ into **Poisson's equation** $\dfrac{\partial^2 \Phi_1}{\partial x^2} = \dfrac{e}{\varepsilon_0} n_{e,1}$:
\begin{equation}
    \frac{\partial^2 \Phi_1}{\partial x^2} = \frac{e^2 n_0}{\omega^2 m_e \varepsilon_0} \frac{\partial^2 \Phi_1}{\partial x^2} - \frac{e T_e}{\omega^2 m_e \varepsilon_0} \underbrace{\left( \frac{\partial^2 n_{e,1}}{\partial x^2} \right)}_{\text{unknown}}
\end{equation}
To eliminate the unknown $\dfrac{\partial^2 n_{e,1}}{\partial x^2}$, we differentiate Poisson's equation twice with respect to $x$:
\begin{equation}
    \frac{\partial^4 \Phi_1}{\partial x^4} = \frac{e}{\varepsilon_0} \frac{\partial^2 n_{e,1}}{\partial x^2} \quad \longrightarrow \quad \frac{\partial^2 n_{e,1}}{\partial x^2} = \frac{\varepsilon_0}{e} \frac{\partial^4 \Phi_1}{\partial x^4}
\end{equation}
Substituting this back into the second-order equation for $\Phi_1$:
\begin{equation}
    \frac{\partial^2 \Phi_1}{\partial x^2} = \underbrace{\left(\frac{e^2 n_0}{m_e \varepsilon_0}\right)}_{\omega_p^2} \frac{1}{\omega^2} \frac{\partial^2 \Phi_1}{\partial x^2} - \frac{\cancel{e} T_e}{\omega^2 m_e \cancel{\varepsilon_0}} \frac{\cancel{\varepsilon_0}}{\cancel{e}} \frac{\partial^4 \Phi_1}{\partial x^4}
\end{equation}

\subsection{Definition of Plasma Frequency $\omega_p$ and Effective Debye Length $\hat{\lambda}_D$}

We define the **electron plasma frequency** $\omega_p$ as:
\begin{equation}
    \boxed{\omega_p^2 = \frac{e^2 n_0}{m_e \varepsilon_0}}
\end{equation}
Recalling the definition of the static Debye length $\dfrac{1}{\lambda_D^2} = \dfrac{e^2 n_0}{\varepsilon_0 T_e}$, we can link $\omega_p$, $\lambda_D$, and the electron thermal speed $v_e = \sqrt{T_e/m_e}$:
\begin{equation}
    \omega_p^2 = \frac{1}{\lambda_D^2} \frac{T_e}{m_e} \quad \implies \quad \frac{T_e}{m_e} = \lambda_D^2 \omega_p^2
\end{equation}
Substituting $\omega_p^2$ and $\dfrac{T_e}{m_e} = \lambda_D^2 \omega_p^2$ into the fourth-order differential equation:
\begin{equation}
    \frac{\partial^2 \Phi_1}{\partial x^2} = \frac{\omega_p^2}{\omega^2} \frac{\partial^2 \Phi_1}{\partial x^2} - \lambda_D^2 \frac{\omega_p^2}{\omega^2} \frac{\partial^4 \Phi_1}{\partial x^4}
\end{equation}
Rearranging to isolate $\dfrac{\partial^4 \Phi_1}{\partial x^4}$:
\begin{equation}
    \frac{\partial^4 \Phi_1}{\partial x^4} = \underbrace{\left( \frac{1 - \left(\dfrac{\omega}{\omega_p}\right)^2}{\lambda_D^2} \right)}_{\equiv \frac{1}{\hat{\lambda}_D^2}} \frac{\partial^2 \Phi_1}{\partial x^2}
\end{equation}
where we have introduced the **effective (dynamic) Debye length** $\hat{\lambda}_D$:
\begin{equation}
    \boxed{\frac{1}{\hat{\lambda}_D^2} = \frac{1 - \left(\dfrac{\omega}{\omega_p}\right)^2}{\lambda_D^2} \quad \Longleftrightarrow \quad \hat{\lambda}_D = \frac{\lambda_D}{\sqrt{1 - \left(\dfrac{\omega}{\omega_p}\right)^2}}}
\end{equation}

\subsection{Integration and Physical Discussion of Dynamic Shielding}

Integrating the fourth-order equation twice with respect to $x$ to remove the $4^\circ$-order derivative:
\begin{equation}
    \frac{\partial^2 \Phi_1}{\partial x^2} = \frac{1}{\hat{\lambda}_D^2} \Phi_1(x) + \cancelto{0}{A x + B}
\end{equation}
Since the dynamic equation must reduce to the static Debye shielding equation in the limit $\omega \to 0$ (i.e., $\hat{\lambda}_D \to \lambda_D$) and the potential vanishes in the unperturbed bulk plasma, the integration constants must vanish ($A = 0$, $B = 0$), yielding:
\begin{equation}
    \boxed{\frac{\partial^2 \Phi_1}{\partial x^2} = \frac{1}{\hat{\lambda}_D^2} \Phi_1(x)}
\end{equation}

\begin{figure}[htbp]
\centering
\begin{tikzpicture}[>=Stealth, scale=1.0]
    % Left Wall
    \draw[thick] (0, 0) -- (0, 3.6);
    \foreach \y in {0.3, 0.9, 1.5, 2.1, 2.7, 3.3} {
        \draw[thin] (-0.4, \y-0.3) -- (0, \y);
    }
    % Right Wall
    \draw[thick] (7.5, 0) -- (7.5, 3.6);
    \foreach \y in {0.3, 0.9, 1.5, 2.1, 2.7, 3.3} {
        \draw[thin] (7.5, \y) -- (7.9, \y-0.3);
    }

    % Horizontal Reference Line
    \draw[thick] (0.2, 1.8) -- (7.3, 1.8);

    % Potential curve \Phi_1(x)
    \draw[very thick, blue!75!black] (0.2, 3.4)
        .. controls (0.35, 2.1) and (0.9, 1.85) .. (1.8, 1.8)
        .. controls (3.0, 1.78) and (4.5, 1.85) .. (5.7, 1.8)
        .. controls (6.6, 1.75) and (7.15, 1.5) .. (7.3, 0.1);

    \node[blue!75!black, font=\large] at (0.9, 3.1) {$\Phi_1$};

    % Tick marks for \hat{\lambda}_D
    \draw[very thick, blue!75!black] (1.8, 1.65) -- (1.8, 1.95);
    \node[blue!75!black, below=0.15cm] at (1.8, 1.65) {$\hat{\lambda}_D$};

    \draw[very thick, blue!75!black] (5.7, 1.65) -- (5.7, 1.95);
    \node[blue!75!black, below=0.15cm] at (5.7, 1.65) {$\hat{\lambda}_D$};

    % Scale bar for static \lambda_D below left wall
    \draw[thick, |-|] (0.1, -0.4) -- (1.2, -0.4);
    \node[below=0.1cm] at (0.65, -0.4) {$\lambda_D$};
\end{tikzpicture}
\caption{Spatial profile of the perturbed potential $\Phi_1(x)$ between two walls, illustrating the enhanced penetration depth $\hat{\lambda}_D > \lambda_D$ in the presence of an AC field.}
\end{figure}

\paragraph{Analysis of Frequency Regimes:}
\begin{enumerate}
    \item \textbf{If $\omega$ is low (with respect to $\omega_p$, i.e., $\omega \ll \omega_p$):}
    \begin{equation}
        1 - \left(\frac{\omega}{\omega_p}\right)^2 \simeq 1 \quad \implies \quad \hat{\lambda}_D \simeq \lambda_D
    \end{equation}
    Standard static Debye shielding is recovered.

    \item \textbf{If $\omega$ increases ($\omega < \omega_p$):}
    \begin{equation}
        1 - \left(\frac{\omega}{\omega_p}\right)^2 < 1 \quad \implies \quad \hat{\lambda}_D > \lambda_D
    \end{equation}
    As the AC field changes faster, the electric field manages to penetrate deeper into the plasma.

    \item \textbf{If $\omega = \omega_p$ (Resonance / Cut-off threshold):}
    \begin{equation}
        1 - \left(\frac{\omega}{\omega_p}\right)^2 = 0 \quad \implies \quad \hat{\lambda}_D = \infty \quad (\textbf{NO SHIELDING})
    \end{equation}
\end{enumerate}

\paragraph{Conditions for Effective Electrostatic and Dynamic Shielding:}
To have a plasma that effectively shields any externally applied electric field, both spatial and temporal criteria must be satisfied:
\begin{equation}
    \boxed{\lambda_D \ll L \quad \text{(static/spatial)}, \qquad \omega \ll \omega_p \quad \text{(dynamic/temporal)}}
\end{equation}

% ============================================================================
\section{Long-Range Coulomb Interactions}
% ============================================================================

\subsection{Course Outline Context}
\begin{enumerate}
    \item Fluid description of plasma
    \item Plasma parameters:
    \begin{enumerate}
        \item $\lambda_D$ (Debye length) $\rightarrow$ Debye shielding and quasi-neutrality
        \item $\omega_p$ (Plasma frequency)
        \item Long-range effects
        \item $\Lambda_D$ (Plasma parameter)
    \end{enumerate}
\end{enumerate}

\subsection{Hard-Sphere Neutral Gas vs.\ Coulomb Interactions in a Plasma}

\begin{itemize}
    \item In a traditional neutral gas, interactions between particles are \emph{``hard-sphere''} collisions: particles must mechanically collide at short range to interact.
    \item In a plasma, interactions are governed by \textbf{Coulomb collisions}, whereby charged particles are continuously deflected or attracted via electrostatic forces.
    \item The Coulomb force that defines this interaction scales with distance $r$ as:
    \begin{equation}
        F_{\text{Coulomb}} \propto \frac{1}{r^2}
    \end{equation}
    \item Because $1/r^2$ decays relatively slowly with increasing distance, there is no sharp cut-off between a particle very close to the test particle and one far away from it. Consequently, we can treat the plasma as a \textbf{smooth, continuous volumetric distribution of charge density $\rho$}.
\end{itemize}

\begin{figure}[htbp]
\centering
\begin{tikzpicture}
    \begin{axis}[
        axis lines=middle,
        xlabel={$r$},
        ylabel={$f(r)$},
        xlabel style={at={(ticklabel* cs:1.02)}, anchor=west},
        ylabel style={at={(ticklabel* cs:1.02)}, anchor=south},
        xmin=0, xmax=4.2,
        ymin=0, ymax=3.2,
        xtick=\empty,
        ytick=\empty,
        samples=100,
        domain=0.45:3.6,
        width=8.5cm,
        height=6cm,
        clip=false
    ]
        % 1/r curve (green)
        \addplot[thick, green!60!black, domain=0.7:3.5] {1.9/x};
        \node[green!60!black, font=\bfseries] at (axis cs:2.7, 1.15) {$1/r$};

        % 1/r^2 curve (red)
        \addplot[thick, red!80!black, domain=0.65:3.2] {1.15/(x^2)};
        \node[red!80!black, font=\bfseries] at (axis cs:1.7, 0.72) {$1/r^2$};

        % 1/r^3 curve (blue)
        \addplot[thick, blue!80!black, domain=0.6:2.8] {0.65/(x^3)};
        \node[blue!80!black, font=\bfseries] at (axis cs:1.05, 0.35) {$1/r^3$};
    \end{axis}
\end{tikzpicture}
\caption{Comparison of radial decay laws $f(r) \propto 1/r$, $1/r^2$, and $1/r^3$, illustrating the relatively slow spatial decay of the Coulomb force ($\propto 1/r^2$).}
\end{figure}

\subsection{Mathematical Proof of Long-Range Collective Interactions}

Let us consider a test point $O$ in a uniform plasma (with constant volumetric charge density $\rho$) where we want to compute the strength of the electric field $E$ generated by the surrounding charges within a conical volume of solid angle $\Omega$.

\begin{figure}[htbp]
\centering
\begin{tikzpicture}[>=Stealth, scale=0.95]
    % --- LEFT DIAGRAM: 3D Sphere with Solid Angle Cone ---
    \begin{scope}[shift={(0,0)}]
        \shade[ball color=gray!15] (0,0) circle (2.1);
        \draw[thick] (0,0) circle (2.1);
        % Equator / Meridian arcs for 3D effect
        \draw[thin, gray!70!black] (-2.1,0) arc (180:360:2.1 and 0.65);
        \draw[thin, dashed, gray!70!black] (2.1,0) arc (0:180:2.1 and 0.65);
        \draw[thin, gray!70!black] (0,-2.1) arc (-90:90:0.75 and 2.1);

        % Solid angle cone inside sphere
        \fill[gray!45, opacity=0.7] (0,0) -- (0.3, 1.85) -- (1.55, 0.95) -- cycle;
        \draw[thick] (0,0) -- (0.3, 1.85);
        \draw[thick] (0,0) -- (1.55, 0.95);
        \draw[thick, fill=gray!25] (0.925, 1.4) ellipse (0.75 and 0.32);
        \draw[dashed, ->, thick] (0,0) -- (1.4, 2.4);

        % Labels inside sphere
        \node at (0.35, 0.7) {$\Omega$};
        \node[left] at (0.15, 1.1) {$r$};
        \node at (1.05, 1.35) {$A$};
        \node[blue!80!black] at (-1.1, 0.9) {$e^-$};
        \node[blue!80!black] at (-1.2, 0.3) {$e^-$};
        \node[red!80!black] at (-0.1, -0.6) {$i^+$};
        \node[red!80!black] at (0.6, -0.5) {$i^+$};
        \node[below, font=\small, align=center] at (0, -2.3) {From 3D sphere to\\1D radial cone};
    \end{scope}

    % --- RIGHT DIAGRAM: 2D Conical Shells ---
    \begin{scope}[shift={(7.8, 0)}]
        % Cone vertex at O (0,0) opening to the left
        \coordinate (O) at (0,0);
        \fill (O) circle (2pt);
        \node[below] at (0, -0.1) {$O$};

        % Electric field arrow to the right
        \draw[->, thick] (O) -- (1.4, 0) node[right] {$E\,?$};

        % Cone rays
        \draw[thick] (O) -- (-4.5, 1.5);
        \draw[thick] (O) -- (-4.5, -1.5);

        % Solid angle arc
        \draw[thick] (-0.8, 0.26) arc (162:198:0.85);
        \node at (-1.15, 0.0) {$\Omega$};

        % Shell 1 at r_1
        \draw[thick] (-2.1, 0.7) arc (161.5:198.5:2.22);
        \draw[thick] (-2.4, 0.8) arc (161.5:198.5:2.53);
        \foreach \y in {-0.6, -0.4, -0.2, 0.0, 0.2, 0.4, 0.6} {
            \draw[thin] (-2.38, \y) -- (-2.12, \y+0.1);
        }
        \node[above] at (-2.2, 0.85) {$dq_1$};
        \node at (-2.75, 0.15) {$S_1$};
        \draw[|-|, thin] (-2.4, -0.95) -- (-2.1, -0.85) node[midway, below] {$dr$};

        % Shell 2 at r_2
        \draw[thick] (-3.8, 1.26) arc (161.5:198.5:4.0);
        \draw[thick] (-4.1, 1.36) arc (161.5:198.5:4.32);
        \foreach \y in {-1.1, -0.85, -0.6, -0.35, -0.1, 0.15, 0.4, 0.65, 0.9, 1.1} {
            \draw[thin] (-4.08, \y) -- (-3.82, \y+0.1);
        }
        \node[above] at (-3.9, 1.4) {$dq_2$};
        \node at (-4.55, 0.0) {$S_2$};
        \draw[|-|, thin] (-4.1, -1.55) -- (-3.8, -1.45) node[midway, below] {$dr$};

        % Radial distance arrows r_1 and r_2
        \draw[->, thin] (0, -0.35) -- (-2.25, -1.1) node[midway, below right] {$r_1$};
        \draw[->, thin] (-2.25, -1.1) -- (-4.1, -1.72) node[midway, below right] {$r_2$};
    \end{scope}
\end{tikzpicture}
\caption{Left: Spherical plasma volume with solid angle $\Omega$ subtending area $A = \Omega r^2$. Right: Conical volume element partitioned into two spherical shells at distances $r_1$ and $r_2$ of equal thickness $dr_1 = dr_2 = dr$.}
\end{figure}

Let us compare the electric field contributions $dE_1$ and $dE_2$ at the test point $O$ produced by two infinitesimal spherical shells of equal radial thickness $dr_1 = dr_2 = dr$ located at distances $r_1$ and $r_2$ ($r_2 > r_1$):
\begin{itemize}
    \item Since the medium is uniform, the volumetric charge density $\rho$ is constant.
    \item The surface area of a spherical cap subtended by the solid angle $\Omega$ at radius $r_k$ is $S_k = \Omega r_k^2$, so the infinitesimal charge in shell $k$ is:
    \begin{equation}
        dq_k = \rho \, dV_k = \rho S_k \, dr = \rho \Omega r_k^2 \, dr
    \end{equation}
    Because $r_2 > r_1$, the volume of shell 2 is larger than that of shell 1 ($dq_2 > dq_1$), meaning more charges reside in the distant shell.
\end{itemize}

Computing the electric field contribution $dE_1$ from the first shell via Coulomb's law:
\begin{equation}
    dE_1 = \frac{1}{4\pi\varepsilon_0} \frac{dq_1}{r_1^2} = \frac{1}{4\pi\varepsilon_0} \frac{\rho S_1 \, dr}{r_1^2} = \frac{\rho \Omega \cancel{r_1^2} \, dr}{\cancel{r_1^2}} \frac{1}{4\pi\varepsilon_0} = \frac{\rho \Omega \, dr}{4\pi\varepsilon_0}
\end{equation}
Similarly, for the second shell at distance $r_2$:
\begin{equation}
    \begin{cases}
        dE_1 = \dfrac{\rho \Omega \, dr}{4\pi\varepsilon_0} \\[1.0em]
        dE_2 = \dfrac{\rho \Omega \, dr}{4\pi\varepsilon_0}
    \end{cases}
    \quad \implies \quad dE_1 = dE_2
\end{equation}

\paragraph{Physical Conclusion:}
As the distance $r$ increases, the Coulomb force per unit charge decreases as $1/r^2$, but the shell volume increases as $r^2$. Consequently, \textbf{distant shells contribute equally to close ones}. In a plasma, \textbf{long-range collective effects dominate over short-range (strong) binary Coulomb collisions}, provided there is a sufficiently large number of particles surrounding the test particle.

% ============================================================================
\section{Definition of the Plasma Parameter $\Lambda_D$}
% ============================================================================

\subsection{Microscopic Length Scales: $b_{\text{part}}$ vs.\ $b_{\text{coul}}$}

For the continuous charge density approximation to hold, the farther we go from a test particle, the enclosed charge must increase smoothly—we need enough particles around our test particle.

Consider two particles in a plasma system, characterized by two fundamental microscopic length scales:
\begin{enumerate}
    \item $b_{\text{coul}}$: the maximum impact distance over which a \textbf{strong binary Coulomb attraction/deflection} occurs between two individual particles.
    \item $b_{\text{part}}$: the average inter-particle separation distance.
\end{enumerate}
For collective long-range effects to dominate over binary collisions, we require:
\begin{equation}
    \boxed{b_{\text{part}} \gg b_{\text{coul}}}
\end{equation}

\begin{figure}[htbp]
\centering
\begin{tikzpicture}[>=Stealth, scale=0.95]
    % --- LEFT DIAGRAM: Test particle, b_coul and b_part ---
    \begin{scope}[shift={(0,0)}]
        % Concentric dashed circles for Coulomb interaction zone
        \draw[dashed, green!50!black, fill=green!10, fill opacity=0.5] (0,0) circle (1.35);
        \draw[dashed, gray] (0,0) circle (0.85);

        % Test particle at origin
        \fill[red!80!black] (0,0) circle (3pt);
        \node[below=0.15cm, font=\small\bfseries] at (0,0) {Test Particle};

        % Radius b_coul
        \draw[<->, thick, green!50!black] (0,0) -- (-0.95, 0.95) node[midway, below left, font=\small] {$b_{\text{coul}}$};
        \node[green!50!black, font=\footnotesize, align=center] at (0, 1.75) {Strong Coulomb\\attraction zone};

        % Second particle at distance b_part
        \fill[blue!80!black] (3.4, 0.4) circle (3pt);
        \node[above=0.1cm, font=\small] at (3.4, 0.4) {Particle};
        \draw[<->, thick] (0.1, 0.05) -- (3.3, 0.38) node[midway, below, font=\small] {$b_{\text{part}}$};
    \end{scope}

    % --- RIGHT DIAGRAM: 3D Unit Cube subdivided into particle cubes ---
    \begin{scope}[shift={(6.8, -1.2)}]
        % Large cube front face
        \draw[thick] (0,0) rectangle (2.6, 2.6);
        % Large cube 3D depth edges
        \draw[thick] (0,2.6) -- (0.9, 3.3) -- (3.5, 3.3) -- (2.6, 2.6);
        \draw[thick] (3.5, 3.3) -- (3.5, 0.7) -- (2.6, 0);

        % Small cube 1 at bottom-left corner
        \draw[thick] (0,0) rectangle (0.65, 0.65);
        \draw[thick] (0,0.65) -- (0.22, 0.82) -- (0.87, 0.82) -- (0.65, 0.65);
        \draw[thick] (0.87, 0.82) -- (0.87, 0.17) -- (0.65, 0);
        \node[font=\small] at (0.32, 0.32) {$1$};

        % Small cube 2 adjacent/shifted
        \draw[thick] (0.87, 0.65) rectangle (1.52, 1.30);
        \draw[thick] (0.87, 1.30) -- (1.09, 1.47) -- (1.74, 1.47) -- (1.52, 1.30);
        \draw[thick] (1.74, 1.47) -- (1.74, 0.82) -- (1.52, 0.65);
        \node[font=\small] at (1.2, 0.97) {$2$};

        \node[right, font=\normalsize] at (1.8, 1.15) {$\times \, n_e$};

        % Scale bar for b_part below small cube 1
        \draw[|-|, thick] (0, -0.35) -- (0.65, -0.35) node[midway, below=0.05cm] {$b_{\text{part}}$};
    \end{scope}
\end{tikzpicture}
\caption{Left: Comparison between the strong Coulomb interaction radius $b_{\text{coul}}$ and the average inter-particle spacing $b_{\text{part}}$. Right: Partition of a unit volume into $n_e$ elementary cubes of volume $V_{\text{part}} = 1/n_e$ and side $b_{\text{part}} = n_e^{-1/3}$.}
\end{figure}

\paragraph{1. Evaluation of $b_{\text{part}}$:}
Dividing the total unit volume $V_{\text{tot}} = 1\text{ m}^3$ by the number density of electrons $n_e$, the elementary volume occupied by a single particle is:
\begin{equation}
    V_{\text{part}} = \frac{V_{\text{tot}}}{\text{\# particles}} = \frac{1}{n_e} \quad \implies \quad b_{\text{part}} = \left(\frac{1}{n_e}\right)^{1/3}
\end{equation}

\paragraph{2. Evaluation of $b_{\text{coul}}$ (Strong Coulomb Collision):}
A strong binary Coulomb deflection occurs when the electrostatic potential energy at closest approach $b_{\text{coul}}$ is comparable to the particle's thermal kinetic energy:
\begin{equation}
    \frac{1}{2} m_e v_e^2 \simeq \frac{e^2}{4\pi\varepsilon_0 b_{\text{coul}}}
\end{equation}
Since $\frac{1}{2} m_e v_e^2 \simeq \frac{1}{2} T_e$:
\begin{equation}
    \frac{\cancel{1}}{\cancel{2}} T_e \simeq \frac{e^2}{\cancel{4}^2 \pi \varepsilon_0 b_{\text{coul}}} \quad \implies \quad b_{\text{coul}} = \frac{e^2}{2\pi\varepsilon_0 T_e}
\end{equation}

\paragraph{3. Imposing $b_{\text{part}} \gg b_{\text{coul}}$:}
\begin{equation}
    \frac{1}{n_e^{1/3}} \gg \frac{e^2}{2\pi\varepsilon_0 T_e}
\end{equation}

\subsection{Derivation of the Debye Sphere Condition $\Lambda_D \gg 1$}

We now introduce the **plasma parameter** $\Lambda_D$, defined as the total number of particles inside a **Debye sphere** of radius $\lambda_D$:
\begin{equation}
    \boxed{\Lambda_D = \frac{4}{3}\pi \lambda_D^3 n_e}
\end{equation}

Cubing both sides of the inequality $b_{\text{part}} \gg b_{\text{coul}}$:
\begin{equation}
    \left(\frac{1}{n_e^{1/3}}\right)^3 \gg \left(\frac{e^2}{2\pi\varepsilon_0 T_e}\right)^3 \quad \implies \quad \frac{1}{n_e} \gg \frac{e^6}{8\pi^3 \varepsilon_0^3 T_e^3}
\end{equation}
Recalling the definition of the Debye length:
\begin{equation}
    \lambda_D^2 = \frac{\varepsilon_0 T_e}{n_e e^2} \quad \longrightarrow \quad \frac{1}{n_e} = \frac{e^2 \lambda_D^2}{\varepsilon_0 T_e}
\end{equation}
Substituting $\dfrac{1}{n_e}$ into the inequality and simplifying common factors:
\begin{equation}
    \frac{\cancel{e^2} \lambda_D^2}{\cancel{\varepsilon_0 T_e}} \gg \frac{e^{\cancel{6}^4}}{8\pi^3 \varepsilon_0^{\cancel{3}^2} T_e^{\cancel{3}^2}} \quad \implies \quad \sqrt{\lambda_D^2} \gg \sqrt{\frac{e^4}{8\pi^3 \varepsilon_0^2 T_e^2}}
\end{equation}
Thus:
\begin{equation}
    \lambda_D \gg \frac{e^2}{2^{3/2}\pi^{3/2}\varepsilon_0 T_e}
\end{equation}
Cubing again and multiplying both sides by $\dfrac{4}{3}\pi n_e$:
\begin{equation}
    \frac{4}{3}\pi n_e \lambda_D^3 \gg \left(\frac{e^2}{2^{3/2}\pi^{3/2}\varepsilon_0 T_e}\right)^3 \frac{4}{3}\pi n_e
\end{equation}
Using $\dfrac{e^2}{\varepsilon_0 T_e} = \dfrac{1}{n_e \lambda_D^2}$ on the right-hand side, this inequality is equivalent to stating that the number of particles in the Debye sphere must be much greater than unity (in a typical fusion plasma, $\Lambda_D \simeq 10^6$ or higher):
\begin{equation}
    \boxed{\Lambda_D \gg 1}
\end{equation}

\subsection{Scaling of $\Lambda_D$ with Electron Temperature $T_e$ and Density $n_e$}

Substituting $\lambda_D = \sqrt{\dfrac{\varepsilon_0 T_e}{n_e e^2}}$ directly into $\Lambda_D = \dfrac{4}{3}\pi \lambda_D^3 n_e$, we obtain the explicit dependence on $T_e$ and $n_e$:
\begin{equation}
    \Lambda_D = \frac{4}{3}\pi \frac{\varepsilon_0^{3/2}}{e^3} \frac{T_e^{3/2}}{n_e^{1/2}} \gg 1
\end{equation}
\emph{(Note: On Slide 31 the denominator is printed as $e^{3/2}$, which is a typographical slip for $(\lambda_D)^3 \propto (e^2)^{-3/2} = e^{-3}$.)}

From this scaling law we deduce two key physical behaviors:
\begin{itemize}
    \item \textbf{Temperature dependence ($\Lambda_D \propto T_e^{3/2}$):}\\
    If $T_e$ increases, since $\lambda_D \propto \sqrt{T_e}$, the Debye length $\lambda_D$ increases too. Consequently, the spherical volume $\frac{4}{3}\pi\lambda_D^3$ in which we count the particles increases, making it \textbf{easier to satisfy the condition $\Lambda_D \gg 1$}.
    \item \textbf{Density dependence ($\Lambda_D \propto n_e^{-1/2}$):}\\
    If $n_e$ increases, since $\lambda_D \propto 1/\sqrt{n_e}$, the Debye length $\lambda_D$ decreases (and $\lambda_D^3 \propto n_e^{-3/2}$ shrinks faster than $n_e$ grows). Thus, the volume in which we count the particles decreases, making it \textbf{more difficult to satisfy the condition $\Lambda_D \gg 1$} $\longrightarrow$ collective plasma behavior is more easily achieved in a relatively low-density, high-temperature plasma.
\end{itemize}

% ============================================================================
\section{Summary: Fundamental Criteria for the Plasma State}
% ============================================================================

To consider a simple ionized gas as a true \textbf{plasma}, three fundamental conditions must be simultaneously satisfied:
\begin{enumerate}
    \item \textbf{Spatial screening condition:}
    \begin{equation}
        \lambda_D \ll L
    \end{equation}
    To achieve electrostatic screening and bulk quasi-neutrality, the characteristic macroscopic size $L$ of the system must be much larger than the Debye length so as to allow charge redistribution.

    \item \textbf{Dynamic screening condition:}
    \begin{equation}
        \omega \ll \omega_p
    \end{equation}
    To allow charged particles (specifically electrons) sufficient time to redistribute and screen out a time-varying electric field, the perturbation frequency $\omega$ must be slow compared to the plasma frequency $\omega_p$.

    \item \textbf{Collective interaction condition:}
    \begin{equation}
        \Lambda_D \gg 1
    \end{equation}
    A sufficiently large number of particles must reside within a Debye sphere so that long-range collective Coulomb effects prevail over short-range binary collisions. This allows the plasma to be treated as a smooth, continuous volumetric distribution of charge density $\rho$.
\end{enumerate}

\end{document}
