# AI Wardrobe

AI Wardrobe is a powerful and easy-to-use service that allows you to virtually try on different outfits on a person's image. This project leverages the capabilities of Generative AI to produce high-quality, realistic images of people wearing different clothes, and offers a variety of options to customize the output. It also includes a pose validation feature to ensure the quality of the input images.

## Features

*   **Virtual Clothes Try-On:** The core feature of the application, allowing you to change the clothing on a person in an image.
*   **Pose Validation:** Analyzes an image to ensure the person's pose is suitable for virtual try-on.
*   **Multiple AI Backends:** Utilizes Google's Gemini model as the primary engine, with a fallback to Vertex AI.
*   **Single and Bulk Processing:** Process images one by one, or in bulk by providing a list of person and clothing image URLs.
*   **Flexible Image Input:** Provide images as base64 encoded strings or as URLs.
*   **Customizable Environments:** Specify a background environment for the output image.
*   **Creative Control:** Add more context to the generated image with parameters like activity, weather, camera, and style.
*   **Simple REST API:** Easy to integrate with your own applications.

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

#### Single Mode (Base64 or URL)

**Request Body:**

| Parameter                  | Type   | Description                                          |
| -------------------------- | ------ | ---------------------------------------------------- |
| `human_image_base64`       | string | Base64 encoded string of the person's image.         |
| `cloth_image_base64`       | string | Base64 encoded string of the clothing image.         |
| `human_image_url`          | string | URL of the person's image.                           |
| `cloth_image_url`          | string | URL of the clothing image.                           |
| `environment_image_base64` | string | Base64 encoded string of the background image.       |
| `activity`                 | string | The activity the person is doing.                    |
| `weather`                  | string | The weather conditions.                              |
| `camera`                   | string | The camera style.                                    |
| `style`                    | string | The overall style of the image.                      |

*Note: You must provide either the base64 encoded images or the image URLs.*

**Success Response:**

A JSON string containing the URL of the generated image.

**Example Request:**

```json
{
    "human_image_url": "https://example.com/person.jpg",
    "cloth_image_url": "https://example.com/cloth.jpg",
    "activity": "riding a bicycle",
    "weather": "sunny, mid-summer",
    "camera": "VHS. VHS artifacts, tracking lines, worn film texture, analog color bleeding, grainy quality",
    "style": "Portrait photography"
}
```

**Example Response:**

```json
"https://your-r2-bucket.your-account.r2.cloudflarestorage.com/path/to/your/image.jpg"
```

#### Bulk Mode

**Request Body:**

| Parameter             | Type         | Description                                    |
| --------------------- | ------------ | ---------------------------------------------- |
| `person_image_urls`   | list[string] | A list of URLs to the person images.           |
| `clothing_image_urls` | list[string] | A list of URLs to the clothing images.         |
| `...`                 | `...`        | Other optional parameters from single mode.    |

**Success Response:**

A JSON list of strings, where each string is a URL to a generated image.

### Pose Validation

*   **Endpoint:** `/api/v1/analyzers/validate-pose/`
*   **Method:** `POST`
*   **Content-Type:** `application/json`

**Request Body:**

| Parameter | Type   | Description                               |
| --------- | ------ | ----------------------------------------- |
| `picture` | string | Base64 encoded string of the image to validate. |

**Success Response:**

```json
{
    "correct": true
}
```

**Error Response:**

```json
{
    "correct": false,
    "message": "<error message>"
}
```