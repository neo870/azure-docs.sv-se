---
title: Skapa PyTorch-modeller med Azure Machine Learning
description: Lär dig hur du kör en nod och distribuerad utbildning av PyTorch-modeller med PyTorch kostnadsuppskattning
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: 27d4ad03e4a7f911fe3c9981618337a2fff51317
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2018
ms.locfileid: "49114625"
---
# <a name="how-to-train-pytorch-models"></a>Hur du tränar PyTorch-modeller

För djupa neurala (DNN) nätverksutbildning med PyTorch, Azure Machine Learning ger en anpassad `PyTorch` klassen för den `Estimator`. Azure SDK `PyTorch` estimator kan du enkelt skicka PyTorch upplärningsjobb för både en nod och distribuerade körs på Azure compute.

## <a name="single-node-training"></a>Nod-utbildning
Utbildning med den `PyTorch` kostnadsuppskattning är ungefär som att använda den [grundläggande `Estimator` ](how-to-train-ml-models.md), så först läsa igenom artikeln och kontrollera att du förstår begreppen som förklaras det.
  
Om du vill köra ett jobb för PyTorch, skapa en instans av en `PyTorch` objekt. Du bör redan har skapat din [beräkningsmålet](how-to-set-up-training-targets.md#batch) objekt `compute_target` och din [datalager](how-to-access-data.md) objektet `ds`.

```Python
from azureml.train.dnn import PyTorch

script_params = {
    '--data_dir': ds
}

pt_est = PyTorch(source_directory='./my-pytorch-proj',
                 script_params=script_params,
                 compute_target=compute_target,
                 entry_script='train.py',
                 use_gpu=True)
```

Här anger vi följande parametrar för PyTorch-konstruktorn:
Parameter | Beskrivning
--|--
`source_directory` |  Lokal katalog som innehåller hela din kod som behövs för utbildningsjobbet. Den här mappen kopieras från din lokala dator till den fjärranslutna beräkningen
`script_params` |  Ordlista att ange kommandoradsargument för att dina utbildningsskript `entry_script`, i form av < kommandoradsargumentet, värde > par
`compute_target` |  Remote beräkning som utbildning skriptet ska köras på, i det här fallet en [Batch AI](how-to-set-up-training-targets.md#batch) kluster
`entry_script` |  FilePath (relativt till den `source_directory`) för utbildning-skriptet som ska köras på den fjärranslutna databearbetning. Den här filen och eventuella ytterligare filer som den är beroende av, bör finnas i den här mappen
`conda_packages` |  Lista över Python-paket installeras via conda som krävs för dina utbildningsskript. Konstruktorn har en annan parameter med namnet `pip_packages` som du kan använda några pip-paket som behövs
`use_gpu` |  Anger denna flagga `True` utnyttja GPU för utbildning. Som standard `False`

Eftersom du använder den `PyTorch` kostnadsuppskattning, behållaren som används för träning innehåller PyTorch-paketet och relaterade beroenden som krävs för utbildning om processorer och GPU: er.

Skicka jobbet PyTorch:
```Python
run = exp.submit(pt_est)
```

## <a name="distributed-training"></a>Distribuerad utbildning
Den `PyTorch` estimator kan du träna dina modeller i hög skala över processor- och GPU-kluster av virtuella Azure-datorer. Du kan enkelt köra distribuerad PyTorch-utbildning med några API-anrop, medan Azure Machine Learning ska hantera i bakgrunden alla infrastruktur och dirigering som behövs för att utföra de här arbetsbelastningarna.

Azure Machine Learning stöder för närvarande MPI-baserade distribuerad utbildning för PyTorch med Horovod framework.

### <a name="horovod"></a>Horovod
[Horovod](https://github.com/uber/horovod) är ett ramverk för öppen källkod ring-allreduce för distribuerad utbildning har utvecklats av Uber.

Om du vill köra distribuerade PyTorch med Horovod framework, skapar du PyTorch-objekt på följande sätt:

```Python
from azureml.train.dnn import PyTorch

pt_est = PyTorch(source_directory='./my-pytorch-project',
                 script_params={},
                 compute_target=compute_target,
                 entry_script='train.py',
                 node_count=2,
                 process_count_per_node=1,
                 distributed_backend='mpi',
                 use_gpu=True)
```

Den här koden visar följande nya parametrar för PyTorch-konstruktorn:
Parameter | Beskrivning | Standard
--|--|--
`node_count` |  Antalet noder som ska användas för utbildning-jobbet. | `1`
`process_count_per_node` |  Antal processer (eller ”anställda”) för att köra på varje nod. | `1`
`distributed_backend` |  Serverdelen för att starta distribuerade utbildning som kostnadsuppskattning erbjuder via MPI.  Att utföra parallella eller distribuerade utbildning (t.ex. `node_count`> 1 eller `process_count_per_node`> 1 eller båda) med MPI (och Horovod), ange `distributed_backend='mpi'`. MPI-implementering som används av Azure Machine Learning är [öppna MPI](https://www.open-mpi.org/). | `None`

Exemplet ovan körs distribuerad utbildning med två arbetsroller en arbetare per nod.

Horovod och dess beroenden kommer att installeras åt dig, så kan du helt enkelt importera det i dina utbildningsskript `train.py` på följande sätt:
```Python
import torch
import horovod
```

Slutligen skicka din distribuerade PyTorch-jobb:
```Python
run = exp.submit(pt_est)
```

## <a name="examples"></a>Exempel
En självstudiekurs om nod PyTorch utbildning, se:
* [Training/01.Train-hyperparameter-Tune-Deploy-with-pytorch](https://github.com/Azure/MachineLearningNotebooks/tree/master/training/01.train-hyperparameter-tune-deploy-with-pytorch)

En självstudiekurs om distribuerade PyTorch med Horovod finns:
* [utbildning/02.distributed-pytorch-med-horovod](https://github.com/Azure/MachineLearningNotebooks/blob/master/training/02.distributed-pytorch-with-horovod)

Hämta dessa anteckningsböcker:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Nästa steg
* [Spåra kör mått vid träning](how-to-track-experiments.md)
* [Justering av hyperparametrar](how-to-tune-hyperparameters.md)
* [Distribuera en tränad modell](how-to-deploy-and-where.md)
