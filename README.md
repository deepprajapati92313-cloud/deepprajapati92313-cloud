#include "clip.h"

namespace egc {

	//Cyrus-Beck clipping algorithm
	//clipWindow specifies the vertices that define the clipping area in conterclockwise order
	//and can represent any convex polygon
	//function returns -1 if the line segment cannot be clipped
	int lineClip_CyrusBeck(std::vector<vec3> clipWindow, vec3& p1, vec3& p2) {
		//TO DO - implement the Cyrus-Beck line clipping algorithm - consult the laboratory work

		if (p1.x == p2.x && p1.y == p2.y)
		{
			return -1;
		}

		float tE = 0.0f;
		float tL = 1.0f;

		float Dx = p2.x - p1.x;
		float Dy = p2.y - p1.y;

		int n = clipWindow.size();
		
		for (int i = 0; i < n; i++)
		{

			vec3 PEi = clipWindow.at(i);
			vec3 nextPEi = clipWindow.at((i+1)%n);

			float Ex = nextPEi.x - PEi.x;
			float Ey = nextPEi.y - PEi.y;


			float Nix = Ey;
			float Niy = -Ex;

			float P0_PEi_x = p1.x - PEi.x;
			float P0_PEi_y = p1.y - PEi.y;

			float PE = (Nix * Dx) + (Niy * Dy);
			float PL = (Nix * P0_PEi_x) + (Niy * P0_PEi_y);

			if (PE != 0.0f)
			{
				float t = PL / (-PE);

				if (PE < 0)
				{
					tE = std::max(tE, t);
				}
				else
				{
					tL = std::min(tL, t);
				}
			}
			else
			{
				if (PL > 0)
					return -1;
			}

		}

		if (tE > tL)
			return -1;
		else
		{
			vec3 origP1 = p1;

			p1.x = origP1.x + Dx * tE;
			p1.y = origP1.y + Dy * tE;
			
			p2.x = origP1.x + Dx * tL;
			p2.y = origP1.y + Dy * tL;
			return 0;
		}

	}

	
}

