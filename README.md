import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException;

class Celula {
    int elemento;
    Celula prox;

    Celula() {
        this(0);
    }

    Celula(int elemento) {
        this.elemento = elemento;
        this.prox = null;
    }
}

class Pilha {
    private Celula topo;

    Pilha() {
        topo = new Celula(); //cabeça
    }

    void empilhar(int x) {
        Celula tmp = new Celula(x);
        tmp.prox = topo.prox;
        topo.prox = tmp;
    }

    int desempilhar() {
        if (topo.prox == null) {
            return -1;
        }

        Celula tmp = topo.prox;
        topo.prox = tmp.prox;
        return tmp.elemento;
    }

    void mostrar() {
        if (topo.prox == null) {
            System.out.println("V");
        } else {
            Celula i = topo.prox;
            String saida = "";
            while (i != null) {
                saida += i.elemento + " ";
                i = i.prox;
            }

            int len = saida.length();
            for (int j = 0; j < len - 1; j++) {
                System.out.print(saida.charAt(j));
            }
            System.out.println();
        }
    }

    char pesquisar(int x) {
        Celula i = topo.prox;
        while (i != null) {
            if (i.elemento == x) {
                return 'S';
            }
            i = i.prox;
        }
        return 'N';
    }
}

public class Main {
    public static String[] split(String linha) {
        int tam = linha.length();
        int qtd = 1;
        for (int i = 0; i < tam; i++) {
            if (linha.charAt(i) == ' ') {
                qtd++;
            }
        }

        String[] partes = new String[qtd];
        int idx = 0;
        String atual = "";

        for (int i = 0; i < tam; i++) {
            char c = linha.charAt(i);
            if (c != ' ') {
                atual += c;
            } else {
                partes[idx] = atual;
                atual = "";
                idx++;
            }
        }

        partes[idx] = atual;
        return partes;
    }

    public static int toInt(String s) {
        int num = 0;
        int sinal = 1;
        int i = 0;

        if (s.charAt(0) == '-') {
            sinal = -1;
            i = 1;
        }

        while (i < s.length()) {
            num = num * 10 + (s.charAt(i) - '0');
            i++;
        }

        return num * sinal;
    }
    public static void main(String[] args) throws IOException {
        Pilha pilha = new Pilha();
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        String linha;
        while ((linha = br.readLine()) != null && linha.length() > 0) {
            String[] partes = split(linha);

            if (partes[0].length() == 1 && partes[0].charAt(0) == 'E') {
                int x = toInt(partes[1]);
                pilha.empilhar(x);
            } else if (partes[0].length() == 1 && partes[0].charAt(0) == 'D') {
                int removido = pilha.desempilhar();
                System.out.println(removido);
            } else if (partes[0].length() == 1 && partes[0].charAt(0) == 'M') {
                pilha.mostrar();
            } else if (partes[0].length() == 1 && partes[0].charAt(0) == 'P') {
                int y = toInt(partes[1]);
                System.out.println(pilha.pesquisar(y));
            }
        }
    }
}
