foi adicionado print do esqueleto do arquivo com css porem com estando nada funcional por enquanto

tive que colocar aqui o codigo porque nao tava conseguinfo porque o git do vscode tava reconhecendo como duas pastas diferentes 

import React, { useState } from 'react';
import { SafeAreaView, Text, TouchableOpacity, View, StyleSheet } from 'react-native';

export default function App() {
  const [expressao, setExpressao] = useState('');
  const [resultado, setResultado] = useState('');

  const botoes = [
    'C', 'DEL', '%', '/',
    '7', '8', '9', '*',
    '4', '5', '6', '-',
    '1', '2', '3', '+',
    '0', '.', '=', '√'
  ];

  const lidarComClique = (valor) => {
    switch (valor) {
      case 'C':
        setExpressao('');
        setResultado('');
        break;
      case 'DEL':
        setExpressao((prev) => prev.slice(0, -1));
        break;
      case '=':
        try {
          const expressaoFormatada = expressao.replace(/√(\d+(\.\d+)?)/g, '($1 ** 0.5)');
          setResultado(String(eval(expressaoFormatada)));
        } catch {
          setResultado('Erro');
        }
        break;
      default:
        if (valor) setExpressao((prev) => prev + valor);
        break;
    }
  };

  return (
    <SafeAreaView style={styles.container}>
      <View style={styles.visor}>
        <Text style={styles.textoExpressao}>{expressao}</Text>
        <Text style={styles.textoResultado}>{resultado}</Text>
      </View>

      <View style={styles.teclado}>
        {botoes.map((botao, index) => (
          <TouchableOpacity
            key={index}
            style={[
              styles.botao,
              ['/', '*', '-', '+', '='].includes(botao) && styles.botaoOperacao,
              ['C', 'DEL'].includes(botao) && styles.botaoAcao,
              !botao && { backgroundColor: 'transparent', elevation: 0 }
            ]}
            onPress={() => lidarComClique(botao)}
            disabled={!botao}
          >
            <Text style={styles.textoBotao}>{botao}</Text>
          </TouchableOpacity>
        ))}
      </View>
    </SafeAreaView>
  );
}

const styles = StyleSheet.create({
  container: {
     marginHorizontal:600,
     marginVertical:20,
    flex: 1,
    backgroundColor: '#ffffff',
    borderRadius:12,
  },
  visor: {
    flex: 1,
    justifyContent: 'flex-end',
    alignItems: 'flex-end',
    padding: 20,
    backgroundColor: '#ffffff',
  },
  textoExpressao: {
    fontSize: 30,
    color: '#000000',
  
  },
  textoResultado: {
    fontSize: 80,
    color: '#000000',
    fontWeight: 'bold',
  },
  teclado: {
    flex: 2,
    flexDirection: 'row',
    flexWrap: 'wrap',
    padding: 10,
    justifyContent: 'space-between',
  },
  botao: {
    width: '22%',
    height: 70,
    backgroundColor: '#d2c6c6',
    justifyContent: 'center',
    alignItems: 'center',
    borderRadius:15,
    marginBottom: 5,
    borderColor:'black',
    borderStyle:'solid'
  },
  botaoOperacao: {
    backgroundColor: '#b19f9f',
  },
  botaoAcao: {
    backgroundColor: '#a89e9e',
  },
  textoBotao: {
    fontSize: 28,
    color: '#000000',
    fontWeight: 'bold',
  }
});
