# Image Object Detection and Indexing for Search

This workflow demonstrates how to build the foundation for a powerful image search service based on object association. It processes a source image, uses a non-LLM vision model from Cloudflare's AI to identify objects, crops each object into a new image, and then indexes these individual object images in Elasticsearch.

## Overview

Traditional image search relies on tags or filenames. [cite_start]This workflow enables a more granular search capability by programmatically identifying and indexing the specific objects *within* an image[cite: 29]. For example, a single image of a street scene could be indexed as separate images of a "car," "person," and "building," making it discoverable through queries for any of those objects.

## Features

* [cite_start]**Object Classification:** Uses the Detr-Resnet-50 model from Cloudflare Workers AI to identify objects and their bounding boxes in an image[cite: 28, 29].
* [cite_start]**Image Cropping:** Automatically crops the identified objects from the source image into new, individual image files[cite: 31].
* **External Storage:** Uploads the newly created object images to a cloud storage provider (Cloudinary in this example).
* [cite_start]**Search Indexing:** Creates a document in Elasticsearch for each cropped object, including the image URL, the object label (e.g., "cat"), and a link back to the original source image[cite: 30, 31].
* **Confidence Filtering:** Includes a filter to only process objects that are identified with a high confidence score (>= 0.9) to ensure accuracy.

## How It Works

1.  **Step 1: Get Source Image:** The workflow starts with a predefined image URL. [cite_start]In a production environment, this could come from various sources like a webhook or a cloud drive[cite: 27].
2.  [cite_start]**Step 2: Object Classification:** The image is sent to the Cloudflare Workers AI API, which returns a list of detected objects, their labels (e.g., "dog"), confidence scores, and bounding box coordinates[cite: 29].
3.  [cite_start]**Step 3: Crop Objects:** For each detected object with a high confidence score, the workflow re-fetches the original image and uses the `Edit Image` node to crop the object based on its bounding box[cite: 30].
4.  **Step 4: Upload and Index:**
    * The cropped image is uploaded to Cloudinary to get a public URL.
    * A new document is created in Elasticsearch. [cite_start]This document contains the Cloudinary URL of the object image, the object's label, and the URL of the original source image[cite: 30].

## Technologies Used

* n8n
* [cite_start]Cloudflare Workers AI (Detr-Resnet-50 model) [cite: 28]
* Cloudinary (for image hosting)
* [cite_start]Elasticsearch (for indexing and search) [cite: 30]

## Setup & Configuration

1.  [cite_start]**Required Variables:** You must set your credentials and configuration in the `Set Variables` node[cite: 25, 31]. This includes:
    * `CLOUDFLARE_ACCOUNT_ID`: Your Cloudflare account ID.
    * `source_image`: The URL of the image you want to process.
    * `elasticsearch_index`: The name of your Elasticsearch index.
2.  **Cloudflare Credentials:** Add your Cloudflare API token to the `Use Detr-Resnet-50 Object Classification` node.
3.  **Cloudinary Credentials:** Configure the `Upload to Cloudinary` node with your Cloudinary API details (this workflow uses query authentication with an upload preset).
4.  **Elasticsearch Credentials:** Add your Elasticsearch credentials to the `Create Docs In Elasticsearch` node.

[Book a chat with me😁](https://cal.com/closegem/coffee-chat)
