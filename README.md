# EEL-PROJECT-1
CODE FOR A real-time location tracking system in C uses a GPS module to collect coordinates, a communication module (Wi-Fi/Bluetooth/GSM) to send data, and a C-based server to receive, process, and store locations. The system includes GPS reading, data encoding, network transmission, server storage, and live map display.
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <math.h>

#define EARTH_RADIUS 6371.0
#ifndef M_PI
#define M_PI 3.14159265358979323846
#endif

/* Function to calculate distance using Haversine formula */
float distanceKM(float lat1, float lon1, float lat2, float lon2)
{
    float dlat = (lat2 - lat1) * M_PI / 180.0;
    float dlon = (lon2 - lon1) * M_PI / 180.0;

    lat1 = lat1 * M_PI / 180.0;
    lat2 = lat2 * M_PI / 180.0;

    float a = sin(dlat/2) * sin(dlat/2) +
              sin(dlon/2) * sin(dlon/2) * cos(lat1) * cos(lat2);

    float c = 2 * atan2(sqrt(a), sqrt(1 - a));

    return EARTH_RADIUS * c;
}

/* GPS Parsing */
void parseGPS(char gpsData[], float *lat, float *lon)
{
    int commaCount = 0, i = 0;
    char latStr[20], lonStr[20];
    int li = 0, lo = 0;

    while (gpsData[i] != '\0')
    {
        if (gpsData[i] == ',')
        {
            commaCount++;
        }
        else if (commaCount == 1)
        {
            latStr[li++] = gpsData[i];
        }
        else if (commaCount == 2)
        {
            lonStr[lo++] = gpsData[i];
        }
        i++;
    }

    latStr[li] = '\0';
    lonStr[lo] = '\0';

    *lat = atof(latStr);
    *lon = atof(lonStr);
}

int main()
{
    char gpsData[50];
    float lat = 0, lon = 0;
    float prevLat = 0, prevLon = 0;
    float totalDistance = 0, dist = 0;

    printf("\n==============================================\n");
    printf("     REAL-TIME LOCATION TRACKING (SIMULATED)\n");
    printf("==============================================\n");

    for (int i = 1; i <= 5; i++)
    {
        printf("\nEnter GPS data like:  $GPS,18.5204,73.8567\n");
        printf("GPS Input %d: ", i);
        scanf("%s", gpsData);

        if (strncmp(gpsData, "$GPS", 4) != 0)
        {
            printf("Invalid Format! Must start with $GPS.\n");
            i--;
            continue;
        }

        parseGPS(gpsData, &lat, &lon);

        printf("\n--- LIVE UPDATE %d ---\n", i);
        printf("Latitude      : %.6f\n", lat);
        printf("Longitude     : %.6f\n", lon);

        if (i > 1)
        {
            dist = distanceKM(prevLat, prevLon, lat, lon);
            totalDistance += dist;

            printf("Distance Moved: %.4f km\n", dist);
            printf("Speed         : %.4f km/s (simulated)\n", dist);
        }
        else
        {
            printf("Starting Point Set.\n");
        }

        prevLat = lat;
        prevLon = lon;

        printf("(Simulating wait...)\n");
    }

    printf("\n==============================================\n");
    printf(" Tracking Finished.\n");
    printf(" Total Distance Travelled: %.4f km\n", totalDistance);
    printf("==============================================\n");

    return 0;
}
<img width="771" height="511" alt="image" src="https://github.com/user-attachments/assets/aab3f12d-86d4-41cd-960b-328bfd5e608d" />
