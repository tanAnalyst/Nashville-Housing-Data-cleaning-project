USE portfolioProject1;   --using database

Select *
From housingData

--------------------------------------------------------------------------------------------------------------------------

-- Standardize Date Format


Select saleDateConverted, CONVERT(Date,SaleDate)
From housingData;


Update housingData
SET SaleDate = CONVERT(Date,SaleDate)

-- If it doesn't Update properly

ALTER TABLE housingData
Add SaleDateConverted Date;

Update housingData
SET SaleDateConverted = CONVERT(Date,SaleDate)


 --------------------------------------------------------------------------------------------------------------------------

-- Populate Property Address data

Select *
From housingData
--Where PropertyAddress is null
order by ParcelID



Select a.ParcelID, a.PropertyAddress, b.ParcelID, b.PropertyAddress, ISNULL(a.PropertyAddress,b.PropertyAddress)
From housingData a
JOIN housingData b
	on a.ParcelID = b.ParcelID
	AND a.[UniqueID ] <> b.[UniqueID ]
Where a.PropertyAddress is null


Update a
SET PropertyAddress = ISNULL(a.PropertyAddress,b.PropertyAddress)
From housingData a
JOIN housingData b
	on a.ParcelID = b.ParcelID
	AND a.[UniqueID ] <> b.[UniqueID ]
Where a.PropertyAddress is null




--------------------------------------------------------------------------------------------------------------------------

-- Breaking out Address into Individual Columns (Address, City, State)


Select PropertyAddress
From housingData
--Where PropertyAddress is null
--order by ParcelID

SELECT
SUBSTRING(PropertyAddress, 1, CHARINDEX(',', PropertyAddress) -1 ) as Address
, SUBSTRING(PropertyAddress, CHARINDEX(',', PropertyAddress) + 1 , LEN(PropertyAddress)) as Address

From housingData


ALTER TABLE housingData
Add PropertySplitAddress Nvarchar(255);

Update housingData
SET PropertySplitAddress = SUBSTRING(PropertyAddress, 1, CHARINDEX(',', PropertyAddress) -1 )


ALTER TABLE housingData
Add PropertySplitCity Nvarchar(255);

Update housingData
SET PropertySplitCity = SUBSTRING(PropertyAddress, CHARINDEX(',', PropertyAddress) + 1 , LEN(PropertyAddress))


--Updating Owner Address (splitting it into 3 different columns i.e address,city,state)

Select
PARSENAME(REPLACE(OwnerAddress, ',', '.') , 3)
,PARSENAME(REPLACE(OwnerAddress, ',', '.') , 2)
,PARSENAME(REPLACE(OwnerAddress, ',', '.') , 1)
From housingData;



ALTER TABLE housingData
Add OwnerSplitAddress Nvarchar(255);
ALTER TABLE housingData
Add OwnerSplitCity Nvarchar(255);
ALTER TABLE housingData
Add OwnerSplitState Nvarchar(255);

Update housingData
SET OwnerSplitAddress = PARSENAME(REPLACE(OwnerAddress, ',', '.') , 3)
Update housingData
SET OwnerSplitCity = PARSENAME(REPLACE(OwnerAddress, ',', '.') , 2)
Update housingData
SET OwnerSplitState = PARSENAME(REPLACE(OwnerAddress, ',', '.') , 1)



Select *
From housingData;

--------------------------------------------------------------------------------------------------------------------------
-- Change Y and N to Yes and No in "Sold as Vacant" field


Select Distinct(SoldAsVacant), Count(SoldAsVacant)
From housingData
Group by SoldAsVacant
order by 2




Select SoldAsVacant
, CASE When SoldAsVacant = 'Y' THEN 'Yes'
	   When SoldAsVacant = 'N' THEN 'No'
	   ELSE SoldAsVacant
	   END
From housingData;


Update housingData
SET SoldAsVacant = CASE When SoldAsVacant = 'Y' THEN 'Yes'
	   When SoldAsVacant = 'N' THEN 'No'
	   ELSE SoldAsVacant
	   END

-----------------------------------------------------------------------------------------------------------------------------------------------------------
-- Remove Duplicates

WITH RowNumCTE AS(
Select *,
	ROW_NUMBER() OVER (
	PARTITION BY ParcelID,
				 PropertyAddress,
				 SalePrice,
				 SaleDate,
				 LegalReference
				 ORDER BY
					UniqueID
					) row_num

From housingData
--order by ParcelID
)
Select *
From RowNumCTE
Where row_num > 1
Order by PropertyAddress


---------------------------------------------------------------------------------------------------------
-- Deleting unused columns

ALTER TABLE housingData
DROP COLUMN OwnerAddress, TaxDistrict, PropertyAddress, SaleDate

Select *
From housingData;












UPDATE try_t
SET ownersplit = STRING_SPLIT(OwnerAddress,',',1);

ALTER TABLE housingData
ADD OwnerSplitAddress Nvarchar(255);
ALTER TABLE housingData
ADD OwnerSplitCity Nvarchar(255);
ALTER TABLE housingData
ADD OwnerSplitState Nvarchar(255);

UPDATE housingData
SET OwnerSplitAddress = PARSENAME(REPLACE(OwnerAddress,',','.'),3);
UPDATE housingData
SET OwnerSplitCity = PARSENAME(REPLACE(OwnerAddress,',','.'),2);
UPDATE housingData
SET OwnerSplitState = PARSENAME(REPLACE(OwnerAddress,',','.'),1);

-------------------------------------------------------------------
--################## Updating the SoldAs vacant column ####################
--replacing y & n with 'Yes' and 'No'

SELECT DISTINCT(SoldAsVacant),COUNT(SoldAsVacant)
FROM housingData
GROUP BY SoldAsVacant;


Update housingData
SET SoldAsVacant = CASE
					WHEN SoldAsVacant='N' THEN 'No'
					WHEN SoldAsVacant='Y' THEN 'Yes'
					END;

