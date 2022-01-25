# Quick start

The purpose of this quick start is to give you a short introduction to the basics of MeiliSearch. We will walk you through setting up MeiliSearch, adding documents, performing your first search, and introduce you to the search preview.

All that is required is a [command line](https://www.learnenough.com/command-line-tutorial#sec-running_a_terminal) for installation, and some way to interact with MeiliSearch afterwards (e.g. [cURL](https://curl.se) or one of our [SDKs](/learn/what_is_meilisearch/sdks.md)).

Let's get started!

## Step 1: Setup and installation

We'll start with downloading and installing MeiliSearch. You have the option to install MeiliSearch locally or deploy it over a cloud service.

### Local installation

:::: tabs

::: tab cURL
Download the **latest stable release** of MeiliSearch with **cURL**.

Launch MeiliSearch to start the server.

```bash
# Install MeiliSearch
curl -L https://install.meilisearch.com | sh

# Launch MeiliSearch
./meilisearch
```

:::

::: tab Homebrew
Download the **latest stable release** of MeiliSearch with **Homebrew**.

Launch MeiliSearch to start the server.

```bash
# Update brew and install MeiliSearch
brew update && brew install meilisearch

# Launch MeiliSearch
meilisearch
```

:::

::: tab Docker
Using **Docker** you can choose to run [any available tag](https://hub.docker.com/r/getmeili/meilisearch/tags).

This command starts the **latest stable release** of MeiliSearch.

```bash
# Fetch the latest version of MeiliSearch image from DockerHub
docker pull getmeili/meilisearch:latest

# Launch MeiliSearch
docker run -it --rm \
    -p 7700:7700 \
    -v $(pwd)/data.ms:/data.ms \
    getmeili/meilisearch:latest
```

Data written to a **Docker container is not persistent** and is deleted along with the container when the latter is stopped. Docker volumes are not deleted when containers are removed. It is then recommended to share volumes between your containers and your host machine to provide persistent storage. MeiliSearch writes data to `/data.ms`

You can learn more about Docker on the [official documentation](https://docs.docker.com/get-docker/).
:::

::: tab APT

Download the **latest stable release** of MeiliSearch with **APT**.

Launch MeiliSearch to start the server.

```bash
# Add MeiliSearch package
echo "deb [trusted=yes] https://apt.fury.io/meilisearch/ /" > /etc/apt/sources.list.d/fury.list

# Update APT and install MeiliSearch
apt update && apt install meilisearch-http

# Launch MeiliSearch
meilisearch
```

:::

::: tab Source

MeiliSearch is written in `Rust`. To compile it, [installing the Rust toolchain](https://www.rust-lang.org/tools/install) is required.

If the Rust toolchain is already installed, clone the repository on your local system and change it to your working directory.

```bash
git clone https://github.com/meilisearch/MeiliSearch
cd MeiliSearch
```

In the cloned repository, compile MeiliSearch.

```bash
# Update the rust toolchain to the latest version
rustup update

# Compile the project
cargo build --release

# Execute the server binary
./target/release/meilisearch
```

:::

::: tab Windows

To install MeiliSearch on Windows, you can:

- use Docker (see "Docker" tab above)
- [download the latest binary](https://github.com/meilisearch/MeiliSearch/releases)
- use the installation script (see "cURL" tab above) if you have installed [Cygwin](https://www.cygwin.com/) or equivalent
- compile from source (see "Source" tab above)

To learn more about the Windows command prompt, follow this [introductory guide](https://www.makeuseof.com/tag/a-beginners-guide-to-the-windows-command-line/).

::::

### Cloud deploy

To deploy MeiliSearch on a cloud service, follow one of our dedicated guides:

- [AWS](/learn/cookbooks/aws.md)
- [DigitalOcean](/learn/cookbooks/digitalocean_droplet.md)
- [Qovery](/learn/cookbooks/qovery.md)

On successfully running MeiliSearch, you should see the following response:

```
888b     d888          d8b 888 d8b  .d8888b.                                    888
8888b   d8888          Y8P 888 Y8P d88P  Y88b                                   888
88888b.d88888              888     Y88b.                                        888
888Y88888P888  .d88b.  888 888 888  "Y888b.    .d88b.   8888b.  888d888 .d8888b 88888b.
888 Y888P 888 d8P  Y8b 888 888 888     "Y88b. d8P  Y8b     "88b 888P"  d88P"    888 "88b
888  Y8P  888 88888888 888 888 888       "888 88888888 .d888888 888    888      888  888
888   "   888 Y8b.     888 888 888 Y88b  d88P Y8b.     888  888 888    Y88b.    888  888
888       888  "Y8888  888 888 888  "Y8888P"   "Y8888  "Y888888 888     "Y8888P 888  888

Database path:       "./data.ms"
Server listening on: "127.0.0.1:7700"
```

Please note that **if you're using a terminal, you will need to open a second window in addition the one running MeiliSearch and use that second window to send the curl commands**.

## Step 2: Add documents

Now that you have [successfully launched a MeiliSearch instance](#step-1-setup-and-installation), the next step is adding some data to search through.

MeiliSearch stores data in the form of discrete records, called [documents](/learn/core_concepts/documents.md). Documents are grouped into large collections, called [indexes](/learn/core_concepts/indexes.md).

To create an index called `movies` and add documents there, run the following command:

<CodeSamples id="getting_started_add_documents_md" />

After adding documents, you should receive a response like this:

```json
{
    "uid": 0,
    "indexUid": "movies",
    "status": "enqueued",
    "type": "documentsAddition",
    "enqueuedAt": "2021-08-11T09:25:53.000000Z"
}
```

Like most database operations in MeiliSearch, document addition is [asynchronous](/learn/advanced/asynchronous_updates.md). This means that the operation will be added to a queue and processed when it reaches the front, rather than happening immediately.

You can use the returned `uid` to [check the status](/reference/api/updates.md) of your document addition request:

<CodeSamples id="getting_started_check_task_status_md" />

If the document addition is successful, your response should look like this:

```json
{
   "uid":0,
   "indexUid":"movies",
   "status":"succeeded",
   "type":"documentAddition",
   "details":{
      "receivedDocuments":19547,
      "indexedDocuments":19547
   },
   "duration":"PT0.030750S",
   "enqueuedAt":"2021-12-20T12:39:18.349288Z",
   "startedAt":"2021-12-20T12:39:18.352490Z",
   "finishedAt":"2021-12-20T12:39:18.380038Z"
}
```

If the task's `status` is still `enqueued` or `processing`, the response may look slightly different. All you have to do is wait a short time and try again. Once the task `status` is `succeeded`, proceed to the next step.

## Step 3: Search

Now that you have MeiliSearch all set up, let's start searching!

<CodeSamples id="getting_started_search_md" />

In the above code sample, the parameter `q` represents the search query. The documents you added in [step two](#step-2-add-documents) will be searched for text that matches `botman`.

MeiliSearch **response**:

```json
{
  "hits": [
    {
      "id": "29751",
      "title": "Batman Unmasked: The Psychology of the Dark Knight",
      "poster": "https://image.tmdb.org/t/p/w1280/jjHu128XLARc2k4cJrblAvZe0HE.jpg",
      "overview": "Delve into the world of Batman and the vigilante justice tha",
      "release_date": "2008-07-15"
    },
    {
      "id": "471474",
      "title": "Batman: Gotham by Gaslight",
      "poster": "https://image.tmdb.org/t/p/w1280/7souLi5zqQCnpZVghaXv0Wowi0y.jpg",
      "overview": "ve Victorian Age Gotham City, Batman begins his war on crime",
      "release_date": "2018-01-12"
    },
    …
  ],
  "offset": 0,
  "limit": 20,
  "processingTimeMs": 2,
  "query": "botman"
}
```

You may notice that by default, MeiliSearch only returns the first 20 results for a search query. This can be changed using the [`limit` parameter](/reference/features/search_parameters.md#limit).

## Step 4: Search preview

MeiliSearch offers an out-of-the-box search preview where you can test it interactively. While MeiliSearch is running, you can access it on your browser at `http://127.0.0.1:7700`.

If your MeiliSearch instance does not have any indexes, you should see this screen.

![no documents](/getting-started/web_interface_without_documents.png)

If you have multiple indexes, you can switch between them using the indexes dropdown.

![multiple indexes](/getting-started/multiple_indexes.png)

## And that's it

You now know all the basics: how to create an index, add documents, check the status of an asynchronous task, and perform a search. To keep going, check out [MeiliSearch 101](/learn/getting_started/chapter_1_filtering_and_sorting.md) for a guided overview of the main features or the [API references](/reference/api/README.md) to dive right in!
