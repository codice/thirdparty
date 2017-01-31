<!--
/*
 * Copyright (c) Codice Foundation
 *
 * This is free software: you can redistribute it and/or modify it under the terms of the GNU Lesser General Public License as published by the Free Software Foundation, either
 * version 3 of the License, or any later version.
 *
 * This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
 * See the GNU Lesser General Public License for more details. A copy of the GNU Lesser General Public License is distributed along with this program and can be found at
 * <http://www.gnu.org/licenses/lgpl.html>.
 */
-->
This tika-bundle is a repackaged Apache Tika OSGi that embeds the full set of OOXML schemas instead of the default POI OOXML subset.
The pom.xml is an altered pom.xml from the tika-bundle in the Apache Tika Project (https://github.com/apache/tika/blob/master/tika-bundle/pom.xml).

## Maintenance
 * When a new version of the tika-bundle is released, determine if the version of Apache POI was changed
   - If the version of POI has been changed, determine if the version of the ooxml-schemas and ooxml-security have changed
 * Change the tika version to the new version
 * If the ooxml-schemas and ooxml-security versions have changed, update them appropriately
 * Determine if the pom.xml for the tika-bundle in the Apache Tika Project has changed
   - If yes, copy / paste the build section of the pom.xml
   - Replace poi-ooxml-schemas with ooxml-schemas and add ooxml-security to the <Embed-Dependency> section