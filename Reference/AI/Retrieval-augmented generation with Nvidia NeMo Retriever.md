Aug 19, 2025

Development Libraries and Frameworks[Development Tools](https://www.infoworld.com/development-tools/)[Generative AI](https://www.infoworld.com/generative-ai/)

## Nvidia’s NeMo Retriever models and RAG pipeline make quick work of ingesting PDFs and generating reports based on them. Chalk one up for the plan-reflect-refine architecture.

![Artificial Intelligence and imagine how it works with icons of people, data science, and machine learning in the background.](https://www.infoworld.com/wp-content/uploads/2025/08/4040473-0-13436600-1755616129-shutterstock_2508362681.jpg?quality=50&strip=all&w=1024)

Credit: Shutterstock

Nvidia was founded by three chip designers (including Jensen Huang, who became CEO) in 1993. By 1997 they had brought a successful high-performance 3D graphics processor to market; two years later the company invented the GPU (graphics processing unit), which caused a sea change in computer graphics, specifically for video games. In 2006 they introduced [CUDA](https://www.infoworld.com/article/2256401/what-is-cuda-parallel-programming-for-gpus.html) (Compute Unified Device Architecture), which allowed them to expand their market to scientific research and general-purpose computing. In 2012 they adapted GPUs to neural networks through specialized CUDA libraries (cuDNN) that could be called from [Python](https://www.infoworld.com/article/2253770/what-is-python-powerful-intuitive-programming.html), which expanded into support for [large language models](https://www.infoworld.com/article/2335213/large-language-models-the-foundations-of-generative-ai.html) (LLMs).

In 2025, along with other products aimed at developers, Nvidia offers an entire suite of enterprise AI software products, including Nvidia NIM, Nvidia NeMo, and Nvidia RAG Blueprint. Together they can import your raw documents, create a vector-indexed knowledge base, and allow you to converse with an AI that knows and can reason from the contents of the knowledge base. Of course, these programs all take full advantage of Nvidia GPUs.

Nvidia NIM is a set of accelerated inference [microservices](https://www.infoworld.com/article/2263327/what-are-microservices-your-next-software-architecture.html) that allow organizations to run AI models on Nvidia GPUs anywhere. Access to NIM generally requires an Nvidia AI Enterprise suite subscription, which typically costs about $4,500 per GPU per year, but the Essentials level of the suite comes gratis for three to five years with some server-class GPUs such as the H200.

Nvidia NeMo is an end-to-end platform for developing custom [generative AI](https://www.infoworld.com/article/2338115/what-is-generative-ai-artificial-intelligence-that-creates.html) including large language models (LLMs), vision language models (VLMs), retrieval models, video models, and speech AI. NeMo Retriever, part of the NeMo platform, provides models for building data extraction and information retrieval pipelines, which extract structured (think tables) and unstructured (think PDF) data from raw documents.

The Nvidia RAG Blueprint demonstrates how to set up a [retrieval-augmented generation](https://www.infoworld.com/article/2335814/what-is-retrieval-augmented-generation-more-accurate-and-reliable-llms.html) (RAG) solution that uses Nvidia NIM and GPU-accelerated components. It provides a quick start for developers to set up a RAG solution using the Nvidia NIM services. The Nvidia AI-Q Research Assistant Blueprint expands on the RAG Blueprint to do deep research and report generation.

## Nvidia AI Enterprise

[Nvidia AI Enterprise](https://docs.nvidia.com/ai-enterprise/index.html#overview) consists of two types of software, application and infrastructure. The application software is for building AI agents, generative AI, and other AI workflows; the infrastructure software includes Nvidia GPU and networking drivers and [Kubernetes](https://www.infoworld.com/article/2266945/what-is-kubernetes-scalable-cloud-native-applications.html) operators.

## Nvidia NIM

[Nvidia NIM](https://developer.nvidia.com/nim?sortBy=developer_learning_library/sort/featured_in.nim:desc,title:asc&hitsPerPage=12) provides containers to self-host GPU-accelerated inferencing microservices for pre-trained and customized AI models. NIM containers improve AI inferencing for AI foundation models on Nvidia GPUs.

## Nvidia NeMo

[NeMo tools and microservices](https://www.nvidia.com/en-us/ai-data-science/products/nemo/) help you to customize and apply AI models from the Nvidia API catalog. Nvidia likes to talk about creating data flywheels with NeMo to continuously optimize AI agents with curated AI and human feedback. NeMo also helps to deploy models with guardrails and retrieval-augmented generation.

## Nvidia NeMo Retriever

[NeMo Retriever](https://developer.nvidia.com/nemo-retriever) microservices accelerate multi-modal document extraction and real-time retrieval with, according to Nvidia, lower RAG costs and higher accuracy. NeMo Retriever supports multilingual and cross-lingual retrieval, and claims to optimize storage, performance, and adaptability for data platforms.

![Nvidia NeMo Retriever AI-Q Blueprint 04](https://b2b-contenthub.com/wp-content/uploads/2025/08/Nvidia-NeMo-Retriever-AI-Q-Blueprint-04-1.png)

NeMo Retriever architecture diagram. Note how many NeMo components interact with each other to create the retriever flow.

Nvidia

## Nvidia AI Blueprints

[Nvidia AI Blueprints](https://github.com/NVIDIA-AI-Blueprints) are reference examples that illustrate how Nvidia NIM can be leveraged to build innovative solutions. There are 18 of them including the RAG Jupyter Notebook we’ll look at momentarily.
## Nvidia Brev

[Nvidia Brev](https://docs.nvidia.com/brev/latest/getting-started.html) is a cloud GPU development platform that allows you to run, build, train, and deploy machine learning models on VMs in the cloud. Launchables are an easy way to share software.

## Nvidia AI-Q Blueprint RAG and Research Assistant reference examples

Nvidia had me use a Brev launchable running on AWS to test out an AI-Q Blueprint reference example.

The Nvidia RAG blueprint (see figure below) serves as a reference solution for a foundational retrieval-augmented generation pipeline. As you probably know, RAG is a pattern that helps LLMs to incorporate knowledge that wasn’t included in their training and to focus on relevant facts; here it is designed to allow users to query an enterprise corpus.

The [Nvidia RAG blueprint](https://github.com/NVIDIA-AI-Blueprints/rag?tab=readme-ov-file#nvidia-rag-blueprint) is a bit more complicated than you might expect, at least partially because it’s trying to handle a variety of input formats, including text, voice, graphics, and formatted pages (e.g. PDFs). It includes refinements such as re-ranking, which narrows relevancy lists; OCR, which extracts text from graphics; and guardrails, which protect against incoming query-based jailbreaks as well as against certain kinds of outgoing hallucinations.

![Nvidia NeMo Retriever AI-Q Blueprint 05](https://b2b-contenthub.com/wp-content/uploads/2025/08/Nvidia-NeMo-Retriever-AI-Q-Blueprint-05.png?w=1024)

Nvidia RAG blueprint [architecture diagram](https://github.com/NVIDIA-AI-Blueprints/rag?tab=readme-ov-file#technical-diagram). Note that this flow includes NeMo Retriever components, LLMs, an OCR component, an object store, and a vector database. The query processing block is based on the [LangChain](https://www.infoworld.com/article/2334784/what-is-langchain-easier-development-of-llm-applications.html) [framework](https://www.langchain.com/).

Nvidia

The Nvidia AI-Q Research Assistant blueprint (see figure below) depends on the RAG blueprint and on an LLM-as-a-judge to verify the relevance of the results. Note the reasoning cycle that occurs prior to final report generation.

![Nvidia NeMo Retriever AI-Q Blueprint 06](https://b2b-contenthub.com/wp-content/uploads/2025/08/Nvidia-NeMo-Retriever-AI-Q-Blueprint-06.png?w=1024)

In addition to RAG functionality, the AI-Q Research Assistant creates a report plan, searches data sources for answers, writes a report, reflects on gaps in the report for further queries, and finishes a report with a list of sources. Note that [Llama models](https://www.infoworld.com/article/3843383/what-is-llama-meta-ais-family-of-large-language-models-explained.html) are used to generate RAG results, to reason on the results, and to generate the report.

Nvidia

The screenshots below are from my runs of the AI-Q Research Assistant blueprint.

![Nvidia NeMo Retriever AI-Q Blueprint 07](https://b2b-contenthub.com/wp-content/uploads/2025/08/Nvidia-NeMo-Retriever-AI-Q-Blueprint-07.png?w=1024)

Nvidia AI-Q Research Assistant blueprint starter page. The bar at the top shows the overall system diagram, and the text below shows a preview of the Jupyter Notebook which appears in the instance. Note that the AI-Q Research Assistant depends on the Nvidia RAG blueprint, shown a few diagrams up from here.

Foundry

![Nvidia NeMo Retriever AI-Q Blueprint 08](https://b2b-contenthub.com/wp-content/uploads/2025/08/Nvidia-NeMo-Retriever-AI-Q-Blueprint-08.png?w=1024)

Starting the AI-Q Research Assistant.

Foundry

![Nvidia NeMo Retriever AI-Q Blueprint 09](https://b2b-contenthub.com/wp-content/uploads/2025/08/Nvidia-NeMo-Retriever-AI-Q-Blueprint-09.png?w=1024)

Running AI-Q Research Assistant instance. The preview of the Jupyter Notebook has been replaced by the instance logs.

Foundry

![Nvidia NeMo Retriever AI-Q Blueprint 10](https://b2b-contenthub.com/wp-content/uploads/2025/08/Nvidia-NeMo-Retriever-AI-Q-Blueprint-10.png?w=1024)

The Jupyter Notebook that appears when the AI-Q Research Assistant is running. The third item down in the directory is what we want.

Foundry

![Nvidia NeMo Retriever AI-Q Blueprint 11](https://b2b-contenthub.com/wp-content/uploads/2025/08/Nvidia-NeMo-Retriever-AI-Q-Blueprint-11.png?w=1024)

The beginning of the “Get Started” Jupyter Notebook for the AI-Q Research Assistant blueprint. The first cell should look familiar from the preview of the launchable.

Foundry

![Nvidia NeMo Retriever AI-Q Blueprint 12](https://b2b-contenthub.com/wp-content/uploads/2025/08/Nvidia-NeMo-Retriever-AI-Q-Blueprint-12.png?w=1024)

Report plan for Amazon 2023 financial performance. You can ask for changes to the plan at this point if you wish.

Foundry

![Nvidia NeMo Retriever AI-Q Blueprint 13](https://b2b-contenthub.com/wp-content/uploads/2025/08/Nvidia-NeMo-Retriever-AI-Q-Blueprint-13.png?w=1024)

Amazon 2023 financial report generated by the AI-Q Research Assistant. You can dive into its process or ask for changes if you wish.

Foundry

## AI-powered research with RAG

The Nvidia AI-Q Research Assistant blueprint I tested did a better job than I expected at ingesting financial reports in PDF form and generating reports based on them in response to user queries. One of the surprises was how well the Llama-based models performed. In separate tests of Llama models in naive RAG designs my results were not nearly as good, so I have to conclude that the plan-reflect-refine architecture helped a lot.

I would have expected my tests of this system to take a day or less. In fact, they took me about a week and half. My first problem turned out to be an error in the documentation. My second problem turned out to be a failure in a back-end process. I was assured that Nvidia has fixed both issues, so that you’re not likely to encounter them.

#### Bottom line

The Nvidia AI-Q Research Assistant blueprint I tested did a better job than I expected at ingesting financial reports in PDF form and generating reports based on them in response to user queries. In separate tests of Llama models in naive RAG designs, my results were not nearly as good. Chalk one up for the plan-reflect-refine architecture.

#### Pros

1. Able to create a credible deep research assistant that can run on-prem or in the cloud
2. Models iterate on the report to refine it
3. NeMo Retriever makes quick work of PDF ingestion
4. Open source blueprint can be adapted to your own AI research applications

#### Cons

1. The version tested still had a few bugs, which should now be fixed
2. Very much tied to Nvidia GPUs

#### Cost

Contact your authorized Nvidia partner for pricing.

#### Platform

Docker Compose or Nvidia AI Workbench, server-class Nvidia GPU(s) with sufficient memory. Can be run on a cloud with Nvidia APIs, or on premises in containers.