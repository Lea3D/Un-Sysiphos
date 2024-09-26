# Unsysiphos

Unsysiphos ist ein einfaches Tool zum Sortieren von Fotos nach ihren EXIF- und IPTC-Metadaten. Es liest Informationen wie die Kameramarke und das Modell aus, um die Fotos automatisch in passende Ordner zu verschieben. Wenn keine EXIF-Daten verfügbar sind, wird auf IPTC-Daten zurückgegriffen.

## Features

- Liest EXIF-Daten aus Bildern, einschließlich Kamera-Marke und Modell.
- Unterstützt mehrere Bildformate (JPEG, PNG, TIFF, HEIC, DNG, etc.).
- Falls keine EXIF-Daten vorhanden sind, werden IPTC-Daten (Titel, Beschreibung) genutzt.
- Erzeugt Ordner basierend auf Kameramarke und Modell, oder verwendet IPTC-Titel als Fallback.
- Unterstützt eine Vielzahl von Dateitypen und Metadatenformaten.

## Installation

1. **Voraussetzungen**: 
   - Python 3.6 oder höher
   - Die folgenden Python-Bibliotheken:
     - Pillow
     - exifread

2. **Installation der Bibliotheken**:
   Führe den folgenden Befehl aus, um die benötigten Bibliotheken zu installieren:
   
   ```bash
   pip install Pillow exifread
