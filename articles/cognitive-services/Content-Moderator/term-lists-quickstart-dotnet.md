---
title: Moderar com as listas de termo personalizadas no Azure Content Moderator | Microsoft Docs
description: Como moderar as listas de termo personalizadas do SDK do Azure Content Moderator para .NET.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/11/2018
ms.author: sajagtap
ms.openlocfilehash: 6da72ad070d9c3a6be38e24626dff77b52fed852
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35363506"
---
# <a name="moderate-with-custom-term-lists-in-net"></a>Moderar com as listas de termos personalizadas no .NET

A lista de termos global padrão do Content Moderator do Azure é suficiente para a maioria das necessidades de moderação de conteúdo. No entanto, você precisará de examinar os termos que são específicos para sua organização. Por exemplo, você talvez queira marcar nomes de concorrentes para análise adicional. 

Você pode usar o SDK do Content Moderator para .NET para criar listas de termos personalizadas para usar com a API de Moderação de Texto.

> [!NOTE]
> Há um limite máximo de **5 listas de termos** com cada lista para **não exceder 10.000 termos**.
>

Este artigo fornece informações e exemplos de código para ajudá-lo a começar a usar o SDK do Content Moderator para .NET para:
- Cria uma lista.
- Adicionar termos a uma lista.
- Examinar termos com os termos em uma lista.
- Excluir termos de uma lista.
- Excluir uma lista.
- Editar informações da lista.
- Atualizar o índice para que as alterações da lista sejam incluídas em uma nova verificação.

Este artigo pressupõe que você já está familiarizado com o Visual Studio e C#.

## <a name="sign-up-for-content-moderator-services"></a>Inscrever-se nos serviços do Content Moderator

Antes de usar os serviços do Content Moderator por meio da API REST ou o SDK, você precisa de uma chave de assinatura.

No painel do Content Moderator, você pode encontrar sua chave de assinatura em **Configurações** > **Credenciais** > **API**  >  **Trial Ocp-Apim-Subscription-Key**. Para mais informações, confira [Visão Geral](overview.md).

## <a name="create-your-visual-studio-project"></a>Criar um projeto do Visual Studio

1. Adicione um novo projeto de **Aplicativo do console (.NET Framework)** à solução.

1. Nomeie o projeto como **TermLists**. Escolha esse projeto como o único projeto de inicialização para a solução.

1. Adicione uma referência para o assembly de projeto **ModeratorHelper** que você criou no [início rápido do auxiliar de cliente do Content Moderator](content-moderator-helper-quickstart-dotnet.md).

### <a name="install-required-packages"></a>Instalar os pacotes necessários

Instale os seguintes pacotes NuGet no projeto TermLists:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Microsoft.Rest.ClientRuntime.Azure
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Atualizar as instruções de uso do programa

Modifique as instruções de uso do programa.

    using System;
    using System.Threading;
    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using ModeratorHelper;

### <a name="add-private-properties"></a>Adicione propriedades privadas

Adicione as seguintes propriedades privadas ao namespace TermLists, classe Programa.

    /// <summary>
    /// The language of the terms in the term lists.
    /// </summary>
    private const string lang = "eng";

    /// <summary>
    /// The minimum amount of time, in milliseconds, to wait between calls
    /// to the Content Moderator APIs.
    /// </summary>
    private const int throttleRate = 3000;

    /// <summary>
    /// The number of minutes to delay after updating the search index before
    /// performing image match operations against a the list.
    /// </summary>
    private const double latencyDelay = 0.5;

## <a name="create-a-term-list"></a>Criar uma lista de termos

Criar uma lista de termos com **ContentModeratorClient.ListManagementTermLists.Create**. O primeiro parâmetro para **Criar** é uma cadeia de caracteres que contém um tipo MIME, que deve ser "application/json". Para obter mais informações, confira a [referência da API](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f). O segundo parâmetro é um objeto de **Corpo** que contém um nome e uma descrição para a lista de novo termo.

Adicione a seguinte definição de método ao namespace TermLists, classe Programa.

> [!NOTE]
> A chave de serviço do Content Moderator tem um limite de taxa RPS (solicitações por segundo) e, se o limite exceder, o SDK lançará uma exceção com um código de erro 429. 
>
> Uma chave de camada gratuita tem um limite de taxa de RPS.

    /// <summary>
    /// Creates a new term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <returns>The term list ID.</returns>
    static string CreateTermList (ContentModeratorClient client)
    {
        Console.WriteLine("Creating term list.");

        Body body = new Body("Term list name", "Term list description");
        TermList list = client.ListManagementTermLists.Create("application/json", body);
        if (false == list.Id.HasValue)
        {
            throw new Exception("TermList.Id value missing.");
        }
        else
        {
            string list_id = list.Id.Value.ToString();
            Console.WriteLine("Term list created. ID: {0}.", list_id);
            Thread.Sleep(throttleRate);
            return list_id;
        }
    }

## <a name="update-term-list-name-and-description"></a>Atualize o nome da lista de termos e descrição

Atualize as informações da lista de termos com **ContentModeratorClient.ListManagementTermLists.Update**. O primeiro parâmetro para **Atualização** é a ID da lista de termos. O segundo parâmetro é um tipo MIME, que deve ser “aplicativo/json”. Para obter mais informações, confira a [referência da API](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f685). O terceiro parâmetro é um objeto de **Corpo** que contém o novo nome e descrição.

Adicione a seguinte definição de método ao namespace TermLists, classe Programa.

    /// <summary>
    /// Update the information for the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to update.</param>
    /// <param name="name">The new name for the term list.</param>
    /// <param name="description">The new description for the term list.</param>
    static void UpdateTermList (ContentModeratorClient client, string list_id, string name = null, string description = null)
    {
        Console.WriteLine("Updating information for term list with ID {0}.", list_id);
        Body body = new Body(name, description);
        client.ListManagementTermLists.Update(list_id, "application/json", body);
        Thread.Sleep(throttleRate);
    }

## <a name="add-a-term-to-a-term-list"></a>Adicionar um termo para uma lista de termos

Adicione a seguinte definição de método ao namespace TermLists, classe Programa.

    /// <summary>
    /// Add a term to the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to update.</param>
    /// <param name="term">The term to add to the term list.</param>
    static void AddTerm (ContentModeratorClient client, string list_id, string term)
    {
        Console.WriteLine("Adding term \"{0}\" to term list with ID {1}.", term, list_id);
        client.ListManagementTerm.AddTerm(list_id, term, lang);
        Thread.Sleep(throttleRate);
    }

## <a name="get-all-terms-in-a-term-list"></a>Obtenha todos os termos para uma lista de termos

Adicione a seguinte definição de método ao namespace TermLists, classe Programa.

    /// <summary>
    /// Get all terms in the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list from which to get all terms.</param>
    static void GetAllTerms(ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Getting terms in term list with ID {0}.", list_id);
        Terms terms = client.ListManagementTerm.GetAllTerms(list_id, lang);
        TermsData data = terms.Data;
        foreach (TermsInList term in data.Terms)
        {
            Console.WriteLine(term.Term);
        }
        Thread.Sleep(throttleRate);
    }

## <a name="add-code-to-refresh-the-search-index"></a>Adicione o código para atualizar o índice de pesquisa

Depois de fazer alterações a uma lista de termos, você pode atualizar seu índice de pesquisa para que as alterações sejam incluídas na próxima vez que você usar a lista de termos de texto na tela. Isso é semelhante a como um mecanismo de pesquisa na área de trabalho (se habilitado) ou um mecanismo de pesquisa da web continuamente atualiza o índice para incluir novos arquivos ou páginas.

Atualizar um índice de pesquisa de lista de termos com **ContentModeratorClient.ListManagementTermLists.RefreshIndexMethod**.

Adicione a seguinte definição de método ao namespace TermLists, classe Programa.

    /// <summary>
    /// Refresh the search index for the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to refresh.</param>
    static void RefreshSearchIndex (ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Refreshing search index for term list with ID {0}.", list_id);
        client.ListManagementTermLists.RefreshIndexMethod(list_id, lang);
        Thread.Sleep((int)(latencyDelay * 60 * 1000));
    }

## <a name="screen-text-using-a-term-list"></a>Texto da tela usando uma lista de termos

Tela de texto usando uma lista de termos com **ContentModeratorClient.TextModeration.ScreenText**, que usa os seguintes parâmetros.

- Idioma dos termos na lista de termos.
- Um tipo MIME, que pode ser "text/html", "texto/xml", "texto/markdown" ou "texto/simples".
- O texto para a tela.
- Um valor booliano. Defina este campo como **verdadeiro** para a correção automática de texto antes de colocar na tela.
- Um valor booliano. Defina este campo como **verdadeiro** para detectar informações de identificação pessoal (PII) no texto.
- ID de Lista de termos.

Para obter mais informações, confira a [referência da API](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f).

**ScreenText** retorna um objeto de **tela**, que tem uma propriedade de **Termos** que lista quaisquer termos que o Content Moderator detectou na tela. Observe que, se o Content Moderator não detectou quaisquer termos durante a triagem de **Termos**, a propriedade terá valor **nulo**.

Adicione a seguinte definição de método ao namespace TermLists, classe Programa.

    /// <summary>
    /// Screen the indicated text for terms in the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to use to screen the text.</param>
    /// <param name="text">The text to screen.</param>
    static void ScreenText (ContentModeratorClient client, string list_id, string text)
    {
        Console.WriteLine("Screening text: \"{0}\" using term list with ID {1}.", text, list_id);
        Screen screen = client.TextModeration.ScreenText(lang, "text/plain", text, false, false, list_id);
        if (null == screen.Terms)
        {
            Console.WriteLine("No terms from the term list were detected in the text.");
        }
        else
        {
            foreach (DetectedTerms term in screen.Terms)
            {
                Console.WriteLine(String.Format("Found term: \"{0}\" from list ID {1} at index {2}.", term.Term, term.ListId, term.Index));
            }
        }
        read.Sleep(throttleRate);
    }

## <a name="delete-terms-and-lists"></a>Excluir termos e listas

Excluir um termo ou uma lista é simples. Você pode usar o SDK para realizar as seguintes tarefas:

- Excluir um termo. (**ContentModeratorClient.ListManagementTerm.DeleteTerm**)
- Excluir todos os termos em uma lista sem excluir a lista. (**ContentModeratorClient.ListManagementTerm.DeleteAllTerms**)
- Excluir uma lista e todo seu conteúdo. (**ContentModeratorClient.ListManagementTermLists.Delete**)

### <a name="delete-a-term"></a>Excluir um termo

Adicione a seguinte definição de método ao namespace TermLists, classe Programa.

    /// <summary>
    /// Delete a term from the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list from which to delete the term.</param>
    /// <param name="term">The term to delete.</param>
    static void DeleteTerm (ContentModeratorClient client, string list_id, string term)
    {
        Console.WriteLine("Removed term \"{0}\" from term list with ID {1}.", term, list_id);
        client.ListManagementTerm.DeleteTerm(list_id, term, lang);
        Thread.Sleep(throttleRate);
    }

### <a name="delete-all-terms-in-a-term-list"></a>Exclua todos os termos para uma lista de termos

Adicione a seguinte definição de método ao namespace TermLists, classe Programa.

    /// <summary>
    /// Delete all terms from the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list from which to delete all terms.</param>
    static void DeleteAllTerms (ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Removing all terms from term list with ID {0}.", list_id);
        client.ListManagementTerm.DeleteAllTerms(list_id, lang);
        Thread.Sleep(throttleRate);
    }

### <a name="delete-a-term-list"></a>Exclua uma lista de termos

Adicione a seguinte definição de método ao namespace TermLists, classe Programa.

    /// <summary>
    /// Delete the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to delete.</param>
    static void DeleteTermList (ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Deleting term list with ID {0}.", list_id);
        client.ListManagementTermLists.Delete(list_id);
        Thread.Sleep(throttleRate);
    }

## <a name="putting-it-all-together"></a>Juntando as peças

Adicione a definição de método **Principal** para a TermLists do namespace, classe de Programa. Por fim, feche a classe Programa e o namespace TermLists.

    static void Main(string[] args)
    {
        using (var client = Clients.NewClient())
        {
            string list_id = CreateTermList(client);

            UpdateTermList(client, list_id, "name", "description");
            AddTerm(client, list_id, "term1");
            AddTerm(client, list_id, "term2");

            GetAllTerms(client, list_id);

            // Always remember to refresh the search index of your list
            RefreshSearchIndex(client, list_id);

            string text = "This text contains the terms \"term1\" and \"term2\".";
            ScreenText(client, list_id, text);

            DeleteTerm(client, list_id, "term1");

            // Always remember to refresh the search index of your list
            RefreshSearchIndex(client, list_id);

            text = "This text contains the terms \"term1\" and \"term2\".";
            ScreenText(client, list_id, text);

            DeleteAllTerms(client, list_id);
            DeleteTermList(client, list_id);

            Console.WriteLine("Press ENTER to close the application.");
            Console.ReadLine();
        }
    }

## <a name="run-the-application-to-see-the-output"></a>Execute o aplicativo de console para ver a saída

A saída será conforme a seguir, mas os dados podem variar.

    Creating term list.
    Term list created. ID: 252.
    Updating information for term list with ID 252.
    
    Adding term "term1" to term list with ID 252.
    Adding term "term2" to term list with ID 252.
    
    Getting terms in term list with ID 252.
    term1
    term2
    
    Refreshing search index for term list with ID 252.
    
    Screening text: "This text contains the terms "term1" and "term2"." using term list with ID 252.
    Found term: "term1" from list ID 252 at index 32.
    Found term: "term2" from list ID 252 at index 46.
    
    Removed term "term1" from term list with ID 252.
    
    Refreshing search index for term list with ID 252.
    
    Screening text: "This text contains the terms "term1" and "term2"." using term list with ID 252.
    Found term: "term2" from list ID 252 at index 46.
    
    Removing all terms from term list with ID 252.
    Deleting term list with ID 252.
    Press ENTER to close the application.
    
## <a name="next-steps"></a>Próximas etapas

[Baixar a solução do Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) para este e outros guias de início rápido do Content Moderator para .NET e começar a integração.