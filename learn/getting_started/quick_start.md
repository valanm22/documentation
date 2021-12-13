# Quick start

This quick start will help you get started with MeiliSearch in just a few steps.

All that is required is a [command line](https://www.learnenough.com/command-line-tutorial#sec-running_a_terminal) for installation, and some way to interact with MeiliSearch afterwards (e.g. [cURL](https://curl.se) or one of our [SDKs](/learn/what_is_meilisearch/sdks.md)). You can find instructions to install each of our SDKs further down in the [add documents section](#step-2-add-documents) of this guide

## Step 1: Install or deploy

We'll start with downloading and installing MeiliSearch. You have the option to install MeiliSearch locally or deploy it over a cloud service.

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

You can communicate with the server through a [RESTful API](/reference/api/README.md) or one of our [SDKs](/learn/what_is_meilisearch/sdks.md).

## Step 2: Add documents

Now that MeiliSearch is up and running, the next step is adding documents. A [document](/learn/core_concepts/documents.md) is an object that contains data in the form of one or more fields. MeiliSearch currently accepts documents in the JSON, NDJSON, and CSV formats.

Your documents are stored in an [index](/learn/core_concepts/indexes.md). If the index does not exist, MeiliSearch creates it when you first add documents.

**All documents must have a [primary key](/learn/core_concepts/documents.md#primary-field).** Each index recognizes only one primary key attribute. Once a primary key has been set for an index, it cannot be changed. If no primary key is found in a document, the document will not be stored.

You can set the primary key manually on index creation or document addition. If no primary key is set, MeiliSearch automatically guesses the primary key when you add documents.

To add documents to an index called `movies`, use:

<CodeSamples id="getting_started_add_documents_md" />

Here's an example of the kind of response you should receive after adding documents.

```json
{
    "uid": 0,
    "indexUid": "movies",
    "status": "enqueued",
    "type": "documentsAddition",
    "enqueuedAt": "2021-08-11T09:25:53.000000Z"
}
```

Document addition is an [asynchronous](/learn/advanced/asynchronous_updates.md) operation. All asynchronous operations return the above response indicating that the operation has been taken into account and will be processed once it reaches the front of the queue.

You can use the `uid` to view additional details on the [task's progress](/reference/api/updates.md).

## Step 3: Search

Now that you have MeiliSearch setup, you can start searching! MeiliSearch [offers many parameters](/reference/features/search_parameters.md) that you can play with to refine your search or change the format of the returned documents. However, by default, the search is already relevant.

<CodeSamples id="getting_started_search_md" />

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

**By default, MeiliSearch returns only the first 20 results for a search query.**

## Step 4: Web interface

MeiliSearch offers an out-of-the-box web interface where you can test MeiliSearch interactively. You can access it on your browser at: `http://127.0.0.1:7700`.

<MovieGif />

If you have multiple indexes, you can switch between them using the indexes dropdown.

![multiple indexes](/getting-started/multiple_indexes.png)

If your MeiliSearch instance does not have any indexes, you should see this screen.

![no documents](/getting-started/web_interface_without_documents.png)

We will be using this interface to demonstrate some features in future chapters.

-> where does the user go next? MeiliSearch 101 for a tour or ?
