# teste-pwc
using System;
using System.Collections.Generic;
using System.Linq;

#region TESTE
var inputsEnderecos = new List<string>
{
    "Miritiba 339",
    "Babaçu 500",
    "Cambuí 804B",
    "Rio Branco 23",
    "Quirino dos Santos 23 b",
    "4, Rue de la République",
    "100 Broadway Av",
    "Calle Sagasta, 26",
    "Calle 44 No 1991",
    "Avenida Paulista, 1991",
    "Avenida Paulista 2999",
    "Avenida Paulista No 2000",
    "Avenida Paulista, No 2001"
};

inputsEnderecos.ForEach(address =>
{
    var resultado = Executar(address);

    if (resultado != null && resultado.Any())
    {
        resultado[0] = resultado[0].Replace(',', char.MinValue);
        resultado[1] = resultado[1].Replace(',', char.MinValue);

        Console.WriteLine($"Endereço: {resultado[0]} || Número: {resultado[1]}");
    }
});

Console.ReadLine();
#endregion

#region MetodoPrincipal
static string[]? Executar(string endereco)
{
    // endereço inválido
    if (string.IsNullOrEmpty(endereco))
        return null;

    string[] resultado = new string[2];
    string[] partes = endereco.Split(' ');

    string parteNumerica = string.Empty;
    string parteEndereco = string.Empty;

    // Verifica se o endereço possui o prefixo "No"
    bool contemPrefixoParaNumero = partes[^2].Contains("No");

    if (contemPrefixoParaNumero)
    {
        resultado[0] = string.Join(" ", partes, 0, partes.Length - 2);
        resultado[1] = string.Join(" ", partes, partes.Length - 2, 2);

        return resultado;
    }

    // Verifica se o endereço possui uma "parte numérica" com letras. Exemplo: Rua Tal 23 b
    bool contemNumeroComLetras = int.TryParse(partes[^2], out int _);

    if (contemNumeroComLetras)
    {
        resultado[0] = string.Join(" ", partes, 0, partes.Length - 2);
        resultado[1] = string.Join(" ", partes, partes.Length - 2, 2);

        return resultado;
    }

    string ultimoElemento = partes[^1];

    // Verifica se o número está no final do endereço e se possui letra no FINAL. Exemplo: Avenida Paulista 804 ou Avenida Paulista 804B
    if (int.TryParse(ultimoElemento.Substring(0, ultimoElemento.Length - 1), out int _))
    {
        resultado[0] = string.Join(" ", partes, 0, partes.Length - 1);
        resultado[1] = ultimoElemento;

        return resultado;
    }

    string primeiroElemento = partes[0];

    // Verifica se o número está no começo do endereço e se possui letra no FINAL. Exemplo: 100 Avenida Paulista ou 100B Avenida Paulista
    if (int.TryParse(primeiroElemento.Substring(0, primeiroElemento.Length - 2), out int _))
        resultado[1] = primeiroElemento;
    else
        resultado[1] = partes[0];

    resultado[0] = string.Join(" ", partes, 1, partes.Length - 1);

    return resultado;
}
#endregion
